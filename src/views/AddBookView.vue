<script lang="ts">
import { defineComponent } from "vue";
import { format } from "date-fns";

export default defineComponent({
  name: "AddBookView",
  data() {
    return {
      title: "",
      author: "",
      publishedDate: "",
      publisher: "",
      description: "",
      coverImage: "",
      ratingAverage: 0,
      ratingCount: 0,
      tags: "",
      initialQty: 0,
      qty: 0,
      error: false,
      errorMsg: "",
    };
  },
  methods: {
    formatDate(date: string): string {
      return format(new Date(date), "dd MMMM yyyy");
    },
    async addBook() {
      const newBook = {
        title: this.title,
        author: this.author,
        publishedDate: this.formatDate(this.publishedDate),
        publisher: this.publisher,
        description: this.description,
        coverImage: this.coverImage,
        rating: {
          average: this.ratingAverage,
          count: this.ratingCount,
        },
        tags: this.tags.split(",").map((tag) => tag.trim()),
        initialQty: this.initialQty,
        qty: this.qty,
      };

      const response = await fetch("http://localhost:3000/book", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(newBook),
      });

      if (response.ok) {
        alert("Book added successfully!");
        this.error = false;
        this.$router.push("/");
      } else {
        this.error = true;
        const data = await response.json();
        if (data.message.includes("duplicate")) {
          this.errorMsg = "Book already exists!";
        } else {
          this.errorMsg = data.message;
        }
      }
    },
  },
});
</script>

<template>
  <main class="pb-24 mx-8 mt-10">
    <h1 class="mb-2 text-3xl font-bold text-center">Add New Book</h1>
    <form
      @submit.prevent="addBook"
      class="max-w-2xl p-8 mx-auto bg-white rounded-lg shadow-md"
    >
      <div class="mb-4">
        <label for="title" class="block mb-2 font-bold text-gray-700"
          >Title</label
        >
        <input
          v-model="title"
          id="title"
          type="text"
          placeholder="Tintin di Tibet"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <div class="mb-4">
        <label for="author" class="block mb-2 font-bold text-gray-700"
          >Author</label
        >
        <input
          v-model="author"
          id="author"
          type="text"
          placeholder="Hergé"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <div class="mb-4">
        <label for="publishedDate" class="block mb-2 font-bold text-gray-700"
          >Published Date</label
        >
        <input
          v-model="publishedDate"
          id="publishedDate"
          type="date"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <div class="mb-4">
        <label for="publisher" class="block mb-2 font-bold text-gray-700"
          >Publisher</label
        >
        <input
          v-model="publisher"
          id="publisher"
          type="text"
          placeholder="Gramedia"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <div class="mb-4">
        <label for="description" class="block mb-2 font-bold text-gray-700"
          >Description</label
        >
        <textarea
          v-model="description"
          id="description"
          placeholder="Kisah petualangan Tintin di Tibet"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        ></textarea>
      </div>
      <div class="mb-4">
        <label for="coverImage" class="block mb-2 font-bold text-gray-700"
          >Cover Image URL</label
        >
        <input
          v-model="coverImage"
          id="coverImage"
          type="url"
          placeholder="https://placehold.co/300x200"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <div class="mb-4">
        <label for="ratingAverage" class="block mb-2 font-bold text-gray-700"
          >Rating Average</label
        >
        <input
          v-model="ratingAverage"
          id="ratingAverage"
          type="number"
          step="0.1"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <div class="mb-4">
        <label for="ratingCount" class="block mb-2 font-bold text-gray-700"
          >Rating Count</label
        >
        <input
          v-model="ratingCount"
          id="ratingCount"
          type="number"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <div class="mb-4">
        <label for="tags" class="block mb-2 font-bold text-gray-700"
          >Tags (comma separated)</label
        >
        <input
          v-model="tags"
          id="tags"
          type="text"
          placeholder="Adventure, Fiction, Mystery"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <div class="mb-4">
        <label for="initialQty" class="block mb-2 font-bold text-gray-700"
          >Initial Quantity</label
        >
        <input
          v-model="initialQty"
          id="initialQty"
          type="number"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <div class="mb-4">
        <label for="qty" class="block mb-2 font-bold text-gray-700"
          >Quantity</label
        >
        <input
          v-model="qty"
          id="qty"
          type="number"
          class="w-full px-3 py-2 text-gray-700 border rounded-lg"
          required
        />
      </div>
      <h5
        class="mb-8 text-xl font-bold text-center text-red-400"
        v-show="error"
      >
        {{ errorMsg }}
      </h5>
      <div class="flex justify-center">
        <button
          type="submit"
          class="px-4 py-2 font-semibold text-white bg-blue-400 rounded-xl"
        >
          Add Book
        </button>
      </div>
    </form>
  </main>
</template>
