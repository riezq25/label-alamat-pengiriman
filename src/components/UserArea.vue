<template>
  <div class="w-full">
    <h3 class="mb-2 text-sm font-semibold capitalize">{{ title }}</h3>
    <div class="grid grid-rows-2 gap-y-2">
      <div class="flex">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="flex-none mr-2 h-4 w-4"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z"
            clip-rule="evenodd"
          />
        </svg>

        <span>{{ nama }}</span>
      </div>

      <div class="flex">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="flex-none mr-2 h-4 w-4"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            d="M2 3a1 1 0 011-1h2.153a1 1 0 01.986.836l.74 4.435a1 1 0 01-.54 1.06l-1.548.773a11.037 11.037 0 006.105 6.105l.774-1.548a1 1 0 011.059-.54l4.435.74a1 1 0 01.836.986V17a1 1 0 01-1 1h-2C7.82 18 2 12.18 2 5V3z"
          />
        </svg>

        <span>+62 {{ phone }}</span>
      </div>

      <div class="flex">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="flex-none mr-2 h-4 w-4"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z"
            clip-rule="evenodd"
          />
        </svg>

        <p class="break-all">
          {{ alamat }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  title: {
    type: String,
    required: true,
    default: "",
  },
  nama: {
    type: String,
    required: true,
    default: "",
  },
  phone: {
    required: true,
    default: "",
  },
  alamat: {
    type: String,
    required: true,
    default: "",
  },
});
</script>