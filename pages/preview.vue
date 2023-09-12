<template>
  <div class="p-5">
    <Navbar />
    <section
      class="h-screen w-screen bg-gradient-to-br from-pink-50 to-indigo-100 p-8"
    >
      <h1 class="text-center font-bold text-2xl text-indigo-500">Preview</h1>

      <div
        class="grid justify-center md:grid-cols-1 lg:grid-cols-3 gap-5 lg:gap-7 my-10"
      >
        <!-- Loop through currentPosts -->
        <div v-for="(post, index) in currentPosts" :key="post.id">
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
      <div class="flex justify-center mt-8">
        <button
          @click="prevPage"
          :disabled="currentPage === 1"
          class="px-4 py-2 mx-2 bg-indigo-500 hover:bg-indigo-600 text-white rounded-md transition duration-300"
        >
          Previous
        </button>
        <button
          @click="nextPage"
          :disabled="currentPage === totalPages"
          class="px-4 py-2 mx-2 bg-indigo-500 hover:bg-indigo-600 text-white rounded-md transition duration-300"
        >
          Next
        </button>
      </div>
    </section>
  </div>
</template>

<script>
import Navbar from '~/components/Navbar.vue'

const dummyData = [
  {
    id: 1,
    title: 'Artikel 1 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 1 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 2,
    title: 'Artikel 2 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 2 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditentukan.',
    category: 'Drafts',
  },
  {
    id: 3,
    title:
      'Artikel 3 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 3 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 4,
    title: 'Artikel 4 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 4 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 5,
    title: 'Artikel 5 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 5 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 6,
    title: 'Artikel 6 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 6 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditentukan.',
    category: 'Drafts',
  },
  {
    id: 7,
    title:
      'Artikel 7 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 7 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 8,
    title: 'Artikel 8 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 8 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 9,
    title: 'Artikel 9 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 9 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 10,
    title: 'Artikel 10 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 10 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 11,
    title:
      'Artikel 11 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 11 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 12,
    title: 'Artikel 12 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 12 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 13,
    title: 'Artikel 13 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 13 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 14,
    title: 'Artikel 14 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 14 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 15,
    title:
      'Artikel 15 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 15 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 16,
    title: 'Artikel 16 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 16 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 17,
    title: 'Artikel 17 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 17 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 18,
    title: 'Artikel 18 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 18 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 19,
    title:
      'Artikel 19 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 19 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 20,
    title: 'Artikel 20 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 20 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 21,
    title: 'Artikel 21 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 21 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 22,
    title: 'Artikel 22 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 22 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 23,
    title:
      'Artikel 23 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 23 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 24,
    title: 'Artikel 24 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 24 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 25,
    title: 'Artikel 25 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 25 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 26,
    title: 'Artikel 26 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 26 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 27,
    title:
      'Artikel 27 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 27 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 28,
    title: 'Artikel 28 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 28 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 29,
    title: 'Artikel 29 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 29 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 30,
    title: 'Artikel 30 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 30 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 31,
    title:
      'Artikel 31 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 31 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 32,
    title: 'Artikel 32 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 32 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 33,
    title: 'Artikel 33 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 33 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 34,
    title: 'Artikel 34 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 34 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 35,
    title:
      'Artikel 35 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 35 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 36,
    title: 'Artikel 36 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 36 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 37,
    title: 'Artikel 37 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 37 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 38,
    title: 'Artikel 38 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 38 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 39,
    title:
      'Artikel 39 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 39 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 40,
    title: 'Artikel 40 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 40 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 41,
    title: 'Artikel 41 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 41 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 42,
    title: 'Artikel 42 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 42 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 43,
    title:
      'Artikel 43 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 43 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 44,
    title: 'Artikel 44 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 44 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 45,
    title: 'Artikel 45 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 45 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 46,
    title: 'Artikel 46 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 46 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 47,
    title:
      'Artikel 47 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 47 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 48,
    title: 'Artikel 48 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 48 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 49,
    title: 'Artikel 49 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 49 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 50,
    title: 'Artikel 50 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 50 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 51,
    title:
      'Artikel 51 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 51 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 52,
    title: 'Artikel 52 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 52 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 53,
    title: 'Artikel 53 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 53 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 54,
    title: 'Artikel 54 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 54 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 55,
    title:
      'Artikel 55 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 55 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 56,
    title: 'Artikel 56 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 56 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 57,
    title: 'Artikel 57 dengan Judul Panjang',
    content:
      'Ini adalah konten dari artikel 57 yang memiliki lebih dari 200 karakter. Konten ini bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal.',
    category: 'Published',
  },
  {
    id: 58,
    title: 'Artikel 58 dengan Judul yang Sangat Panjang Sekali',
    content:
      'Ini adalah konten dari artikel 58 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan.',
    category: 'Drafts',
  },
  {
    id: 59,
    title:
      'Artikel 59 dengan Judul yang Super Panjang dan Lebih dari 20 Karakter',
    content:
      'Ini adalah konten dari artikel 59 yang memiliki lebih dari 200 karakter. Konten ini juga bisa menjadi lebih panjang lagi untuk memenuhi persyaratan minimal yang telah ditetapkan. Sekarang, kita menambahkan beberapa kalimat lagi untuk memastikan kontennya cukup panjang.',
    category: 'Published',
  },
  {
    id: 60,
    title: 'Artikel 60 dengan Judul yang Melebihi Batasan Minimum',
    content:
      'Ini adalah konten dari artikel 60 yang memiliki lebih dari 200 karakter. Konten ini mencakup lebih banyak teks daripada yang diperlukan untuk memenuhi persyaratan minimum yang telah ditetapkan.',
    category: 'Drafts',
  },
]

// Lanjutkan dengan data lainnya sesuai kebutuhan Anda

export default {
  components: {
    Navbar,
  },
  data() {
    return {
      posts: dummyData, // Data dummy
      currentPage: 1,
      itemsPerPage: 9,
    }
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
