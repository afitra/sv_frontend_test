<template>
  <div class="p-5">
    <Navbar @noedit-event="handleNoEdit" />
    <div v-if="isEdit">
      <h2 class="text-2xl font-semibold mb-4">Edit Post</h2>
      <Form :isEdit="isEdit" :idEdit="idEdit" @afterEdit-event="handleNoEdit" />
    </div>
    <div v-if="!isEdit">
      <!-- Tabs -->
      <ul class="flex space-x-4">
        <li
          @click="changeTab('publish')"
          :class="{ 'border-b-2 border-blue-500': activeTab === 'publish' }"
        >
          <a class="block py-2 text-blue-500 font-semibold">Published</a>
        </li>
        <li
          @click="changeTab('draft')"
          :class="{ 'border-b-2 border-blue-500': activeTab === 'draft' }"
        >
          <a class="block py-2 text-gray-500">Drafts</a>
        </li>
        <li
          @click="changeTab('thrash')"
          :class="{ 'border-b-2 border-blue-500': activeTab === 'thrash' }"
        >
          <a class="block py-2 text-gray-500">Trashed</a>
        </li>
      </ul>

      <div class="text-center text-gray-600 mt-2" v-if="isLoading">
        Loading...
      </div>
      <!-- Table -->
      <table
        class="w-full mt-4 bg-white border border-gray-300"
        v-if="!isLoading"
      >
        <thead>
          <tr>
            <th class="px-4 py-2">Index</th>
            <th class="px-4 py-2">Title</th>
            <th class="px-4 py-2">Category</th>
            <th class="px-4 py-2">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(post, index) in currentPosts" :key="post.id">
            <td class="px-4 py-2">
              {{ `${currentPage - 1}${index + 1}` }}
            </td>
            <td class="px-4 py-2">{{ post.title }}</td>
            <td class="px-4 py-2">{{ post.category }}</td>
            <td class="px-4 py-2">
              <button
                class="px-4 py-2 bg-blue-500 text-white rounded"
                @click="editPost(post.id)"
              >
                Edit
              </button>
              <button
                class="px-4 py-2 bg-yellow-400 text-white rounded"
                @click="moveToTrash(post.id)"
                v-if="activeTab != 'thrash'"
              >
                Trash
              </button>
            </td>
          </tr>
        </tbody>
      </table>

      <!-- Pagination -->
      <div class="flex justify-center mt-8 pb-10"></div>
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
  </div>
</template>

<script>
import Swal from 'sweetalert2'

import Navbar from '~/components/Navbar.vue'
import Form from '~/components/Form.vue'


export default {
  components: {
    Navbar,
    Form,
  },
  data() {
    return {
      activeTab: 'publish',
      posts: [], // Data dummy
      currentPage: 1,
      itemsPerPage: 10,
      isLoading: true,
      isEdit: false,
      idEdit: 0,
    }
  },
  mounted() {
    this.fetchData('publish')
  },
  computed: {
    currentPosts() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage
      const endIndex = startIndex + this.itemsPerPage
      return this.posts.slice(startIndex, endIndex)
    },
    totalPages() {
      return Math.ceil(this.posts.length / this.itemsPerPage)
    },
  },
  methods: {
    handleNoEdit(flag) {
      this.isEdit = flag
      if (flag == false) {
        this.fetchData(this.activeTab)
      }
    },

    async fetchData(status) {
      try {
        this.isLoading = true
        let url = `/article/status/${status}`
        const response = await this.$axios.get(url)

        if (response.data.code == '99') {
          return Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Something went wrong!',
          })
        }

        this.isLoading = false
        this.posts = response.data.data
        return Swal.fire('Success', 'Data Loaded!', 'success')
      } catch (error) {
        Swal.fire({
          icon: 'error',
          title: 'Oops...',
          text: 'Something went wrong!',
        })
      }
    },
    changeTab(tab) {
      this.activeTab = tab
      this.fetchData(tab)
      this.currentPage = 1
      // Filter posts based on the selected tab (published, drafts, trashed)
    },
    editPost(postId) {
      this.isEdit = true
      this.idEdit = postId
      // Redirect to edit page with postId
    },
    async moveToTrash(postId) {
      // Move the post to the trash

      try {
        const dataPost = (await this.$axios.get(`/article/${postId}`)).data.data
        dataPost.status = 'thrash'
        const response = await this.$axios.put(
          `/article/status/${postId}/thrash`,
          dataPost
        )
        if (response.data.code == '99') {
          return Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Something went wrong!',
          })
        }
        this.fetchData(this.activeTab)
        return Swal.fire('Success', 'Data saved!', 'success')
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
