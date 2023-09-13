<template>
  <div class="p-5">
    <Navbar />
    <section
      class="h-screen w-screen bg-gradient-to-br from-pink-50 to-indigo-100 p-8"
    >
      <h1 class="text-center font-bold text-2xl text-indigo-500">Preview</h1>
      <div class="text-center text-gray-600 mt-2" v-if="isLoading">
        Loading...
      </div>
      <div
        class="grid justify-center md:grid-cols-1 lg:grid-cols-3 gap-5 lg:gap-7 my-10"
        v-if="!isLoading"
      >
        <!-- Loop through currentPosts -->
        <div v-for="(post, index) in posts" :key="index">
          <!-- Card -->
          <div
            class="bg-white rounded-lg border shadow-md max-w-xs md:max-w-none overflow-hidden"
          >
            <img
              class="h-56 lg:h-60 w-full object-cover"
              src="https://images.unsplash.com/photo-1523289217630-0dd16184af8e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxzZWFyY2h8OHx8d29tZW4lMjBlbXBvd2VybWVudHxlbnwwfHwwfHw%3D&auto=format&fit=crop&w=500&q=60"
              alt=""
            />
            <div class="p-3">
              <span class="text-sm text-primary">November 19, 2022</span>
              <h3 class="font-semibold text-xl leading-6 text-gray-700 my-2">
                {{ post.title }}
              </h3>
              <p class="paragraph-normal text-gray-600">
                {{ post.content }}
              </p>

              <a class="mt-3 block" href="#">{{ post.category }}</a>
            </div>
          </div>
        </div>
      </div>
      <!-- Pagination Centered and Styled -->
      <div class="flex justify-center mt-8 pb-10">
        <button
          @click="prevPage()"
          :disabled="currentPage === 1"
          class="px-4 py-2 mx-2 bg-indigo-500 hover:bg-indigo-600 text-white rounded-md transition duration-300"
        >
          Previous
        </button>
        <button
          @click="nextPage()"
          :disabled="currentPage * itemsPerPage < totalPages"
          class="px-4 py-2 mx-2 bg-indigo-500 hover:bg-indigo-600 text-white rounded-md transition duration-300"
        >
          Next
        </button>
      </div>
    </section>
  </div>
</template>

<script>
import Swal from 'sweetalert2'
import Navbar from '~/components/Navbar.vue'

export default {
  components: {
    Navbar,
  },
  data() {
    return {
      posts: [], // Data dummy
      currentPage: 1,
      itemsPerPage: 9,
      isLoading: true,
      totalPages: 0,
    }
  },
  mounted() {
    this.fetchData()
  },
  watch: {
    currentPage: function (val) {
      this.fetchData()
    },
  },
  computed: {
    currentPosts() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage
      const endIndex = startIndex + this.itemsPerPage
      return this.posts.slice(startIndex, endIndex)
    },
  },
  methods: {
    async fetchData() {
      try {
        this.isLoading = true
        let url = `/article/${this.itemsPerPage}/${
          this.currentPage * this.itemsPerPage - this.itemsPerPage
        }`

        if (this.currentPage == 1) {
          url = `/article/${this.itemsPerPage}/0`
        }
        const response = await this.$axios.get(url)

        if (response.data.code == '99') {
          return Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Something went wrong!',
          })
        }
        this.isLoading = false
        this.posts = response.data.data.post
        this.totalPages = Math.ceil(
          response.data.data.count / this.itemsPerPage
        )
        return Swal.fire('Success', 'Data Loaded!', 'success')
      } catch (error) {
        Swal.fire({
          icon: 'error',
          title: 'Oops...',
          text: 'Something went wrong!',
        })
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++
      }
    },
  },
}
</script>
