<template>
  <div class="min-h-screen min-w-full bg-neutral-200 p-8 print:p-0">
    <div
      class="
        md:max-w-3xl
        bg-white
        shadow-md
        print:shadow-none
        p-4
        print:p-0
        rounded-lg
        mx-auto
        grid
        sm:grid-cols-1
        md:grid-cols-2
        gap-4
        print:gap-0 print:mx-0
      "
    >
      <div>
        <div class="print:hidden w-full grid grid-cols-1 gap-4">
          <div class="form-input">
            <label class="" for="from">Nama Pengirim</label>
            <input
              class=""
              type="text"
              id="from"
              v-model="data.pengirim.nama"
            />
          </div>

          <div class="form-input">
            <label class="" for="phone">Telepon Pengirim</label>
            <input
              class=""
              type="number"
              maxlength="13"
              minlength="8"
              min="1"
              id="phone"
              v-model.number="data.pengirim.phone"
            />
          </div>

          <div class="form-input">
            <label class="" for="alamat">Alamat Pengirim</label>

            <textarea
              id="alamat"
              rows="3"
              v-model="data.pengirim.alamat"
            ></textarea>
          </div>

          <div>
            <input
              type="file"
              name=""
              id="excel-selector"
              @change="readExcel"
              accept=".xls, .xlsx"
            />
          </div>

          <div class="flex justify-center text-white text-sm gap-4">
            <button @click="deletePengirim" class="btn btn-danger">
              Hapus Pengirim
            </button>
            <button
              @click="saveLokal('pengirim', data.pengirim)"
              class="btn btn-primary"
            >
              Simpan Pengirim
            </button>
          </div>

          <div
            class="flex justify-center text-white text-sm gap-4"
            v-if="data.excel"
          >
            <button @click="reset" class="btn btn-secondary">Reset</button>
            <button @click="print('label')" class="btn btn-primary">
              Print
            </button>
          </div>

          <div class="flex items-center space-x-1 mx-auto" v-if="data.excel">
            <button
              @click="prev"
              :disabled="data.current == 0"
              class="btn-pagination"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="w-6 h-6"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M11 17l-5-5m0 0l5-5m-5 5h12"
                />
              </svg>
            </button>

            <div>
              <input
                @change="toPage"
                type="number"
                min="1"
                class="text-center w-12 rounded-md"
                :max="data.excel.length"
                :value="data.current + 1"
              />

              <span> dari </span>

              <input
                disabled
                type="number"
                min="1"
                class="text-center w-12 rounded-md"
                :value="data.excel.length"
              />
            </div>

            <button
              @click="next"
              :disabled="data.excel.length == data.excel.length - 1"
              class="btn-pagination"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="w-6 h-6"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M13 7l5 5m0 0l-5 5m5-5H6"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>
      <div
        class="p-4 w-full mx-auto print:p-0 print:mx-0 bg-gray-500 rounded-lg"
      >
        <!-- 58 30 -->
        <div
          class="
            w-[300px]
            h-[580px]
            mx-auto
            print:mx-0
            p-2
            border-2
            rounded-lg
            print:rounded-none print:border-0
          "
        >
          <div
            class="
              w-full
              h-full
              bg-white
              border-2 border-dashed border-black
              flex flex-col
              justify-between
              p-4
              font-mono
              text-black text-xs
              uppercase
              tracking-wider
            "
          >
            <div
              class="
                flex flex-col
                divide-y-2 divide-dashed divide-black
                space-y-4
              "
            >
              <div class="w-full">
                <h3 class="mb-2 text-sm font-semibold capitalize">
                  Metode Pengiriman
                </h3>
                <span>{{
                  data.penerima.kurir ? data.penerima.kurir : "-"
                }}</span>
              </div>

              <!-- Penerima -->

              <user-area
                title="penerima"
                :nama="data.penerima.nama"
                :alamat="data.penerima.alamat"
                :phone="data.penerima.phone"
              />

              <!-- Pengirim -->

              <user-area
                title="pengirim"
                :nama="data.pengirim.nama"
                :alamat="data.pengirim.alamat"
                :phone="data.pengirim.phone"
              />
            </div>

            <div class="flex flex-col gap-y-10">
              <div class="flex flex-col text-sm">
                <span class="font-semibold">Total</span>
                <span class="capitalize"
                  >Rp
                  {{ data.penerima.total ? data.penerima.total : "0" }},-</span
                >
              </div>

              <span class="text-center text-xs"
                >Terima kasih atas pesanan Anda di toko kami.</span
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from "vue";
import UserArea from "./components/UserArea.vue";
import XLSX from "xlsx";

const print = (id) => {
  window.print();
};

const data = reactive({
  pengirim: {
    nama: "",
    alamat: "",
    phone: "",
  },
  penerima: { nama: "", alamat: "", phone: "", total: "", kurir: "" },
  excel: null,
  current: 0,
});

const readLokal = (key) => {
  if (localStorage.getItem(key)) {
    return JSON.parse(localStorage.getItem(key));
  }
  return { nama: "", alamat: "", phone: "" };
};

const saveLokal = (key, val) => {
  localStorage.setItem(key, JSON.stringify(val));
};

const deletePengirim = () => {
  data.pengirim = { nama: "", alamat: "", phone: "" };
  localStorage.removeItem("pengirim");
};

const readExcel = (e) => {
  data.current = 0
  var fileReader = new FileReader();
  fileReader.onload = function (event) {
    var workbook = XLSX.read(event.target.result, {
      type: "binary",
    });
    workbook.SheetNames.forEach((sheet) => {
      let rowObject = XLSX.utils.sheet_to_row_object_array(
        workbook.Sheets[sheet]
      );
      data.excel = rowObject;
      console.log(rowObject);
      setPenerima(0);
    });
  };
  fileReader.readAsBinaryString(e.target.files[0]);
};

const setPenerima = (i) => {
  data.penerima.nama = data.excel[i].nama;
  data.penerima.total = data.excel[i].bayar;
  data.penerima.phone = data.excel[i]["no hp"];
  data.penerima.alamat = data.excel[i].alamat;
  data.penerima.kurir = data.excel[i].kurir;
};

const next = () => {
  if (data.current < data.excel.length - 1) {
    data.current++;
    setPenerima(data.current);
  }
};
const toPage = (e) => {
  const val = e.target.value;

  if (val > 0 && val < data.excel.length - 1) {
    data.current = val - 1;
    setPenerima(data.current);
  }
};

const prev = () => {
  if (data.current >= 0) {
    data.current--;
    setPenerima(data.current);
  }
};
const reset = () => {
  data.current = 0;
  data.excel = null;
  data.penerima = { nama: "", alamat: "", phone: "", total: "", kurir: "" };
  document.getElementById("excel-selector").value = "";
};

data.pengirim = readLokal("pengirim");
</script>

<style>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}

.btn {
  @apply py-2
                px-4
                rounded-lg;
}

.btn-pagination {
  @apply flex
                items-center
                px-4
                py-2
                text-gray-500
                bg-gray-300
                hover:bg-blue-400 hover:text-white
                rounded-md;
}

.btn-danger {
  @apply text-white
                bg-red-600
                active:ring-4 active:ring-red-200 active:bg-red-700
                hover:bg-red-700;
}

.btn-primary {
  @apply text-white
                bg-blue-600
                active:ring-4 active:ring-blue-200 active:bg-blue-700
                hover:bg-blue-700;
}

.btn-secondary {
  @apply text-gray-800  bg-gray-200
                active:ring-4 active:ring-gray-100 active:bg-gray-300
                hover:bg-gray-300;
}

.form-input label {
  @apply block mb-1 text-sm font-semibold;
}

.form-input input,
textarea {
  @apply rounded-lg w-full;
}
</style>