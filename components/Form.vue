<template>
  <div>
    <div class="text-center text-gray-600 mt-2" v-if="isLoading">
      Loading...
    </div>
    <div v-if="!isLoading">
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
          @click="requestPost('publish')"
        >
          Publish
        </button>
        <button
          class="px-4 py-2 bg-yellow-400 text-white rounded"
          @click="requestPost('draft')"
        >
          Draft
        </button>
      </div>
    </div>
  </div>
</template>
<script>
import Swal from 'sweetalert2'
export default {
  components: {},
  props: {
    isEdit: {
      type: Boolean,
      default: false,
    },
    idEdit: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      isLoading: true,
      form: {
        title: 'ini dummy data  dari pada ngetik 123456789123456789',
        content:
          'ini dummy data dari pada ngetik     Throughout time, randomness was generated through mechanical devices such as dice, coin flips, and playing cards. A mechanical method of achieving randomness can be more time and resource consuming especially',
        category: 'ini dummy data dari pada ngetik category1', // Default category
        status: 'publish', // Default status
      },
    }
  },
  mounted() {
    this.fetchData()
  },
  methods: {
    async fetchData() {
      if (this.isEdit) {
        try {
          this.isLoading = true
          const response = await this.$axios.get(`/article/${this.idEdit}`)
          this.isLoading = false
          this.form = response.data.data
        } catch (error) {
          Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Something went wrong!',
          })
        }
      } else {
        this.isLoading = false
      }
    },
    async requestPost(status) {
      try {
        this.form.status = status
        let method = 'post'
        let url = '/article'
        if (this.isEdit) {
          method = 'put'
          url = `/article/${this.idEdit}`
        }
        const response = await this.$axios[method](url, this.form)
        if (response.data.code == '99') {
          return Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Something went wrong!',
          })
        }
        this.form = {}
        this.$emit('afterEdit-event', false)

        await Swal.fire('Success', 'Data saved!', 'success')
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
