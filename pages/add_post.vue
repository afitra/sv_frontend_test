<template>
  <div class="p-5">
    <Navbar />
    <h2 class="text-2xl font-semibold mb-4">Add New Post</h2>
    <div>
      <div class="mb-4">
        <label for="title" class="block text-gray-600">Title</label>
        <input
          v-model="form.title"
          type="text"
          id="title"
          name="title"
          class="w-full px-3 py-2 border border-gray-300 rounded"
        />
      </div>
      <div class="mb-4">
        <label for="content" class="block text-gray-600">Content</label>
        <textarea
          v-model="form.content"
          id="content"
          name="content"
          class="w-full px-3 py-2 border border-gray-300 rounded"
        ></textarea>
      </div>
      <div class="mb-4">
        <label for="category" class="block text-gray-600">Category</label>
        <input
          v-model="form.category"
          type="text"
          id="category"
          name="category"
          class="w-full px-3 py-2 border border-gray-300 rounded"
        />
      </div>
      <div class="mb-4">
        <button
          class="px-4 py-2 bg-blue-500 text-white rounded"
          @click="addPost('Publish')"
        >
          Publish
        </button>
        <button
          class="px-4 py-2 bg-yellow-400 text-white rounded"
          @click="addPost('Draft')"
        >
          Draft
        </button>
      </div>
    </div>
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
      form: {
        title: 'tes_title_deh123456789123456789',
        content:
          'Throughout time, randomness was generated through mechanical devices such as dice, coin flips, and playing cards. A mechanical method of achieving randomness can be more time and resource consuming especially',
        category: 'category1', // Default category
        status: 'Thrash', // Default status
      },
    }
  },
  methods: {
    async addPost(status) {
      this.form.status = status
      try {
        const response = await this.$axios.post('/article', this.form)
        if (response.data.code == '99') {
          return Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Something went wrong!',
          })
        }
        this.form = {}
        return Swal.fire('Success', 'Data saved!', 'success')
      } catch (error) {
        Swal.fire({
          icon: 'error',
          title: 'Oops...',
          text: 'Something went wrong!',
        })
      }
    },
  },
}
</script>
