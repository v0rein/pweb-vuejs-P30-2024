<script lang="ts">
import { defineComponent } from "vue";

interface Rating {
  average: number;
  count: number;
}

interface BookDetail {
  rating: Rating;
  _id: string;
  title: string;
  author: string;
  publishedDate: string;
  publisher: string;
  description: string;
  coverImage: string;
  tags: string[];
  initialQty: number;
  qty: number;
  createdAt: string;
  updatedAt: string;
  __v: number;
}

export default defineComponent({
  name: "DetailBookView",
  data: () => ({
    bookDetail: {
      _id: "",
      title: "",
      author: "",
      publishedDate: "",
      publisher: "",
      description: "",
      coverImage: "",
      tags: [],
      initialQty: 0,
      qty: 0,
      createdAt: "",
      updatedAt: "",
      __v: 0,
      rating: {
        average: 0,
        count: 0,
      },
    } as BookDetail,
    fetchError: false,
  }),
  async created() {
    try {
      const response = await fetch(
        `http://localhost:3000/book/${this.$route.params.id}`
      );
      if (!response.ok) {
        throw new Error("Failed to fetch book data");
      }
      const data = await response.json();
      console.log(data);
      this.bookDetail = { ...data };
      console.log(this.bookDetail);
    } catch (error) {
      console.error(error);
      this.fetchError = true;
    }
  },
  methods: {
    async deleteBook() {
      const response = await fetch(
        `http://localhost:3000/book/${this.$route.params.id}`,
        {
          method: "DELETE",
        }
      );
      const data = await response.json();
      console.log(data);
      alert("Book has been removed!");
      this.$router.push("/");
    },
  },
  computed: {
    starRating(): string {
      const stars = Math.floor(this.bookDetail.rating.average);
      return "⭐".repeat(stars);
    },
  },
});
</script>

<template>
  <main class="mx-8 mt-14 pb-14">
    <RouterLink
      to="/"
      class="inline-block px-4 py-2 font-semibold text-white bg-blue-400 lg:ml-24 rounded-xl"
      >⬅️ {{ !!bookDetail.title }}</RouterLink
    >
    <div v-if="fetchError" class="mt-8">
      <h1 class="text-3xl font-bold text-center">Failed to load book data</h1>
    </div>

    <div v-else-if="bookDetail.title" class="mt-8">
      <div class="flex flex-col lg:ml-24 gap-x-10 lg:flex-row">
        <div class="w-full md:w-4/6 lg:w-[500px] lg:flex-shrink-0">
          <img
            :src="bookDetail.coverImage"
            class="w-full rounded-xl"
            alt="Book Cover"
          />
        </div>
        <div class="mt-10 lg:mt-0 lg:pr-24">
          <h1 class="text-xl font-bold text-left md:text-2xl lg:text-3xl">
            Buku {{ bookDetail.title }} by {{ bookDetail.author }}
          </h1>
          <h5 class="mt-2 text-sm font-bold text-gray-500">
            {{ bookDetail.rating.average }} {{ starRating }} ({{
              bookDetail.rating.count
            }})
          </h5>
          <hr class="my-2 border border-black" />
          <h3 class="text-left text-md md:text-lg">
            <span class="font-bold">About:</span> {{ bookDetail.description }}
          </h3>
          <h3 class="text-left text-md md:text-lg">
            <span class="font-bold">Published:</span>
            {{ bookDetail.publishedDate }} by
            {{ bookDetail.publisher }}
          </h3>
          <h3 class="text-left text-md md:text-lg">
            <span class="font-bold">Category:</span>
            {{ bookDetail.tags.join(", ") }}
          </h3>
          <h3 class="text-left text-md md:text-lg">
            <span class="font-bold">Stock:</span>
            {{ bookDetail.qty }} of {{ bookDetail.initialQty }} books
          </h3>
        </div>
      </div>
      <div class="flex items-center justify-center lg:ml-24">
        <button
          @click="deleteBook"
          class="inline-block w-1/2 px-4 py-2 mt-8 font-semibold text-white bg-red-400 rounded-xl"
        >
          Remove Book
        </button>
      </div>
    </div>
    <div v-else class="mt-8">
      <h1 class="text-3xl font-bold text-center">Loading...</h1>
    </div>
  </main>
</template>
