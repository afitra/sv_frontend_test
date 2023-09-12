<template>
  <nav class="border-gray-200 px-2 mb-10">
    <div class="container mx-auto flex flex-wrap items-center justify-between">
      <div
        id="mobile-menu-3"
        class="hidden md:flex justify-between items-center w-full md:w-auto md:order-1"
      >
        <ul
          class="flex-col md:flex-row flex md:space-x-8 mt-4 md:mt-0 md:text-sm md:font-medium"
        >
          <li>
            <a
              aria-current="page"
              class="bg-blue-700 md:bg-transparent text-white block pl-3 pr-4 py-2 md:text-blue-700 md:p-0 rounded"
              @click="changePage('/all_post')"
              >All Post</a
            >
          </li>
          <li>
            <a
              class="text-gray-700 hover:bg-gray-50 border-b border-gray-100 md:hover:bg-transparent md:border-0 block pl-3 pr-4 py-2 md:hover:text-blue-700 md:p-0"
              @click="changePage('/add_post')"
              >Add Post</a
            >
          </li>
          <li>
            <a
              class="text-gray-700 hover:bg-gray-50 border-b border-gray-100 md:hover:bg-transparent md:border-0 block pl-3 pr-4 py-2 md:hover:text-blue-700 md:p-0"
              @click="changePage('/preview')"
              >Preview</a
            >
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>
<script>
export default {
  methods: {
    changePage(uri) {
      console.log('>>> ', uri)
      this.$router.push({
        path: uri,
      })
    },
  },
}
</script>
