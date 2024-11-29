<script lang="ts">
import { RouterLink } from "vue-router";
import type { PropType } from "vue";

export interface BookObject {
  _id: string;
  title: string;
  author: string;
  publishedDate: string;
  tags: string[];
  initialQty: number;
  qty: number;
  publisher: string;
}

export default {
  name: "BookCard",
  components: {
    RouterLink,
  },
  props: {
    book: {
      type: Object as PropType<BookObject>,
      required: true,
    },
  },
  methods: {
    getReadMoreLink(id: string) {
      return `/detail/${id}`;
    },
  },
};
</script>

<template>
  <div class="px-6 pt-6 pb-8 border border-black rounded-xl">
    <img
      src="https://placehold.co/300x200"
      alt="Wajib Pake Typescript hehe"
      class="w-full mb-4 rounded-xl"
    />
    <h3 class="text-xl font-bold text-left">
      {{ book.title }} | {{ book.author }}
    </h3>
    <hr class="my-2 border border-black" />
    <h5 class="font-medium text-left text-md">
      Published: {{ book.publishedDate }} by {{ book.publisher }}
    </h5>
    <h5 class="font-medium text-left text-md">
      Category: {{ book.tags.join(", ") }}
    </h5>
    <h5 class="font-medium text-left text-md">
      Quantity: {{ book.qty }} of {{ book.initialQty }} books
    </h5>
    <RouterLink
      :to="getReadMoreLink(book._id)"
      class="inline-block px-4 py-2 mt-2 font-semibold text-white bg-blue-400 rounded-xl"
      >Read More</RouterLink
    >
  </div>
</template>
