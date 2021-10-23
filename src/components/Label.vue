<script setup>
import { ref } from "vue";
import CheckIcon from "./components/CheckIcon.vue";

const checkTop = ref(0);
const checkLeft = ref(0);

const from = ref("");
const phone = ref("");

const check = (top, left) => {
  if (checkTop.value == top && checkLeft.value == left) {
    checkTop.value = 0;
    checkLeft.value = 0;
  } else {
    checkTop.value = top;
    checkLeft.value = left;
  }
};
</script>

<template>
  <div class="bg-slate-300 min-h-screen min-w-full p-8">
    <div class="p-8 bg-white rounded-lg max-w-3xl mx-auto">
      <div class="print:hidden mb-8">
        <div class="flex p-2 gap-8">
          <div class="w-full">
            <label for="from" class="input-label">Nama Pengirim</label>
            <input
              type="text"
              name="from"
              id="from"
              v-model="from"
              autocomplete="given-name"
              class="input"
            />
          </div>

          <div class="w-full">
            <label for="phone" class="input-label">Nomor Telfon Pengirim</label>
            <input
              type="text"
              name="phone"
              id="phone"
              v-model="phone"
              autocomplete="given-name"
              class="input"
            />
          </div>
        </div>
      </div>

      <div
        class="
          relative
          w-[700px]
          overflow-hidden
          mx-auto
          font-medium
          text-lg text-gray-800
        "
      >
        <img
          src="/public/label.jpg"
          alt="label"
          class="object-cover w-full h-full"
        />

        <div
          class="absolute w-10"
          :class="['top-[' + checkTop + 'rem]', 'left-[' + checkLeft + 'rem]']"
        >
          <CheckIcon />
        </div>

        <button
          @click="check(2, 4.5)"
          class="absolute top-[2rem] left-[4.5rem] w-36 h-16"
        ></button>

        <button
          @click="check(2, 15.3)"
          class="absolute top-[2rem] left-[15.3rem] w-36 h-16"
        ></button>

        <button
          @click="check(2, 26.1)"
          class="absolute top-[2rem] left-[26.1rem] w-52 h-16"
        ></button>

        <span class="absolute top-[7.3rem] left-[10rem]">
          Rizqi Ari Putra
        </span>
        <span class="absolute top-[8.7rem] left-[10rem]">089524067884</span>
        <span class="absolute top-[10.2rem] left-[10rem] w-8/12 leading-tight"
          >Jln RE Martadinata Gang Mawar VIII No 166 (Belakang SPBU) Kampung Opas Kel. Opas Indah Kec. Tamansari, Kota. Pangkalpinang, Prov. Kepulauan Bangka Belitung
</span
        >
        <span class="absolute top-[17.8rem] left-[10rem]">{{ from }}</span>
        <span class="absolute top-[19.2rem] left-[10rem]">{{ phone }}</span>
      </div>
    </div>
  </div>
</template>


<style>
.input {
  @apply mt-1
                focus:ring-indigo-500 focus:border-indigo-500
                block
                w-full
                shadow-sm
                sm:text-sm
                border-gray-300
                rounded-md;
}

.input-label {
  @apply block text-sm font-medium text-gray-700;
}
</style>
