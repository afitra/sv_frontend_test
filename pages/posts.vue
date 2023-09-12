<template>
 <div>

   <!-- Navbar.vue -->
   <div class="bg-blue-500 p-4 text-white">
     <div class="container mx-auto">
       <h1 class="text-2xl font-semibold">Your Blog</h1>
     </div>
   </div>

   <!-- Content -->
   <div class="container mx-auto p-4">

     <!-- Tabs -->
     <ul class="flex space-x-4">
       <li class="border-b-2 border-blue-500">
         <a href="#" class="block py-2 text-blue-500 font-semibold">Published</a>
       </li>
       <li>
         <a href="#" class="block py-2 text-gray-500">Drafts</a>
       </li>
       <li>
         <a href="#" class="block py-2 text-gray-500">Trashed</a>
       </li>
     </ul>

     <!-- Table -->
     <table class="w-full mt-4 bg-white border border-gray-300">
       <thead>
       <tr>
         <th class="px-4 py-2">Title</th>
         <th class="px-4 py-2">Category</th>
         <th class="px-4 py-2">Action</th>
       </tr>
       </thead>
       <tbody>
       <!-- Data rows go here -->
       </tbody>
     </table>

   </div>

   <!-- Add New Post Form -->
   <div class="container mx-auto p-4 mt-8 bg-white">
     <h2 class="text-2xl font-semibold mb-4">Add New Post</h2>
     <form>
       <div class="mb-4">
         <label for="title" class="block text-gray-600">Title</label>
         <input type="text" id="title" name="title" class="w-full px-3 py-2 border border-gray-300 rounded">
       </div>
       <div class="mb-4">
         <label for="content" class="block text-gray-600">Content</label>
         <textarea id="content" name="content" class="w-full px-3 py-2 border border-gray-300 rounded"></textarea>
       </div>
       <div class="mb-4">
         <label for="category" class="block text-gray-600">Category</label>
         <select id="category" name="category" class="w-full px-3 py-2 border border-gray-300 rounded">
           <option value="category1">Category 1</option>
           <option value="category2">Category 2</option>
           <!-- Add more categories here -->
         </select>
       </div>
       <div class="mb-4">
         <button type="submit" class="px-4 py-2 bg-blue-500 text-white rounded">Publish</button>
         <button type="submit" class="px-4 py-2 bg-gray-400 text-white rounded">Draft</button>
       </div>
     </form>
   </div>

   <!-- Preview -->
   <div class="container mx-auto p-4 mt-8 bg-white">
     <h2 class="text-2xl font-semibold mb-4">Preview</h2>
     <!-- Pagination and post previews go here -->
   </div>

 </div>
</template>
<script setup>
</script>
