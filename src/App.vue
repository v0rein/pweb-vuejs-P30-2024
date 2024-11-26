<script lang="ts">
import { defineComponent } from "vue";
import { RouterLink, RouterView } from "vue-router";

export default defineComponent({
  name: "App",
  data() {
    return {
      isMenuOpen: false,
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
    closeMenu() {
      this.isMenuOpen = false;
    },
  },
  components: {
    RouterLink,
    RouterView,
  },
});
</script>

<template>
  <nav class="p-4 bg-gray-800">
    <div class="container flex items-center justify-between mx-auto">
      <div class="flex items-center space-x-4">
        <h3 class="font-bold text-white">Book App PWEB A | P30</h3>
      </div>
      <div class="md:hidden">
        <button @click="toggleMenu" class="text-white focus:outline-none">
          <svg
            class="w-6 h-6"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 6h16M4 12h16m-7 6h7"
            ></path>
          </svg>
        </button>
      </div>
      <div class="items-center hidden space-x-4 md:flex">
        <RouterLink to="/" class="text-white">Home</RouterLink>
        <RouterLink to="/add-book" class="text-white">Add New Book</RouterLink>
      </div>
    </div>
  </nav>

  <div
    :class="{
      'translate-x-full': !isMenuOpen,
      'translate-x-0': isMenuOpen,
    }"
    class="fixed top-0 right-0 w-64 h-full text-white transition-transform duration-300 ease-in-out transform bg-gray-800 md:hidden"
  >
    <button
      @click="closeMenu"
      class="absolute text-white top-4 right-4 focus:outline-none"
    >
      <svg
        class="w-6 h-6"
        fill="none"
        stroke="currentColor"
        viewBox="0 0 24 24"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M6 18L18 6M6 6l12 12"
        ></path>
      </svg>
    </button>
    <div class="flex flex-col items-center mt-16 space-y-4">
      <RouterLink to="/" class="text-white" @click="closeMenu">Home</RouterLink>
      <RouterLink to="/add-book" class="text-white" @click="closeMenu"
        >Add New Book</RouterLink
      >
    </div>
  </div>

  <RouterView />
</template>
