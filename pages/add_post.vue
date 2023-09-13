<template>
  <div class="p-5">
    <Navbar />
    <h2 class="text-2xl font-semibold mb-4">Add New Post</h2>
    <Form :isEdit="editMode" />
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
      editMode: false,
    }
  },
}
</script>
