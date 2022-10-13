<script>
import moment from "moment";
import useValidate from "@vuelidate/core";
import {
  required,
  email,
  minLength,
  sameAs,
  numeric,
} from "@vuelidate/validators";
import { reactive, computed, ref } from "vue";
import { mapState } from "vuex";
moment().format("h:mm:ss a");

export default {
  setup() {
    const state = reactive({
      email: "",
      password: {
        password: "",
        confirm: "",
      },
      telp: "",
      nik: "",
      namalengkap: "",
      tempatlahir: "",
      answer: "",
    });

    const rules = computed(() => {
      return {
        email: { required, email },
        password: {
          password: { required, minLength: minLength(6) },
          confirm: { required, sameAs: sameAs(state.password.password) },
        },
        telp: { required, numeric },
        nik: { required, numeric, minLength: minLength(16) },
        namalengkap: { required },
        tempatlahir: { required },
        answer: { required, numeric },
      };
    });

    const v$ = useValidate(rules, state);

    const randomNumber1 = ref(Math.ceil(Math.random() * 10));
    const randomNumber2 = ref(Math.ceil(Math.random() * 10));

    const handleCaptcha = () => {
      randomNumber1.value = Math.ceil(Math.random() * 10);
      randomNumber2.value = Math.ceil(Math.random() * 10);
    };

    return {
      handleCaptcha,
      state,
      v$,
      randomNumber1,
      randomNumber2,
    };
  },

  methods: {
    submitForm() {
      this.v$.$validate();
      if (!this.v$.$error) {
        alert("Berhasil mendaftar");
      } else {
        alert("Silahkan cek inputan anda lagi");
      }
    },
  },
  // computed: {
  //   ...mapState({
  //     nama: this.v$.namalengkap,
  //   }),
  // },
};
</script>

<template>
  <div class="h-full px-2 sm:px-0">
    <div class="h-full sm flex justify-between">
      <div class="content pt-24 max-w-450 mx-auto relative">
        <nav class="text-lg font-semibold mb-5">
          <ul class="nav-list">
            <li class="hover:font-bold"><a href="">Beranda</a></li>
            <li class="hover:font-bold">Informasi</li>
            <li class="hover:font-bold">Aplikasi UT Lainnya</li>
          </ul>
        </nav>
        <form class="text-center block">
          <div class="text-2xl">Sistem Informasi Akademik</div>
          <div class="text-2xl text-secondary-blue-dark font-bold">
            UNIVERSITAS TERBUKA
          </div>
          <div class="py-2 text-primary-default font-bold">
            Pendaftaran Mahasiswa Baru
          </div>

          <div class="md:container md:mx-auto">
            <div class="mt-4 block">
              <div class="text-left text-neutral-5">Jenjang Pendidikan</div>
              <div class="flex flex-wrap mt-2 text-neutral-5">
                <div class="mr-5">
                  <input
                    type="radio"
                    name="jenjangPendidikan"
                    id="Diploma"
                    class="mr-1 transform scale-125 cursor-pointer"
                  />
                  <label for="Diploma" class="cursor-pointer"
                    >Diploma dan Sarjana</label
                  >
                </div>
                <div class="mr-5">
                  <input
                    type="radio"
                    name="jenjangPendidikan"
                    id="Magister"
                    class="mr-1 transform scale-125 cursor-pointer"
                    value="magister"
                  /><label for="Magister" class="cursor-pointer"
                    >Magister</label
                  >
                </div>
                <div class="mr-5">
                  <input
                    type="radio"
                    name="jenjangPendidikan"
                    id="Doktoral"
                    class="mr-1 transform scale-125 cursor-pointer"
                    value="magister"
                  />
                  <label for="Doktoral" class="cursor-pointer">Doktoral</label>
                </div>
              </div>
            </div>

            <div
              class="flex place-items-center bg-white px-2 transition-all border-2 border-transparent rounded-t-lg mt-4 font-bold"
              title="Masukkan Nama Lengkap Sesuai Akta Lahir"
            >
              <input
                placeholder="Nama Lengkap"
                type="text"
                name=""
                v-model="namalengkap"
                class="upper-case pr-3 py-2 outline-none w-full"
              />
            </div>
            <span
              v-if="v$.namalengkap.$error"
              class="text-status-error text-left text-sm mt-1"
            >
              Nama harus diisi
            </span>

            <div class="w-full sm:inline-flex sm:space-x-2">
              <div
                class="flex place-items-center mt-1 bg-white px-2 transition-all border-2 border-transparent w-full sm:w-1/2"
                title="Masukkan Tempat Lahir Sesuai Akta Lahir"
              >
                <input
                  placeholder="Tempat Lahir"
                  type="text"
                  name=""
                  v-model="tempatlahir"
                  class="pl-2 pr-3 py-2 outline-none w-full"
                />
              </div>
              <input
                class="bg-white border mt-1 px-3 relative text-base w-full sm:w-1/2 border-transparent"
                placeholder="Tanggal Lahir"
                type="date"
                title="Masukkan Tanggal Lahir Sesuai Akta Lahir"
              />
            </div>

            <div
              class="flex place-items-center bg-white px-2 transition-all border-2 border-transparent mt-1 font-bold"
              title="Masukkan Nama Lengkap Sesuai Akta Lahir"
            >
              <input
                placeholder="-- Masukkan Agama --"
                type="text"
                name=""
                class="pr-3 py-2 outline-none w-full dropdown-toggle"
              />
            </div>

            <div class="w-full my-1 text-neutral-5">
              <table class="text-left font-bold text-sm mt-2">
                <tr class="grid grid-cols-3 gap-4 py-1">
                  <td>Jenis Kelamin</td>
                  <td>
                    <input
                      type="radio"
                      name="jenisKelamin"
                      id="jenisKelaminA"
                      value="0"
                      class="mr-2 transform scale-125"
                    />
                    <label for="jenisKelaminA" class="cursor-pointer"
                      >Perempuan</label
                    >
                  </td>
                  <td>
                    <input
                      type="radio"
                      name="jenisKelamin"
                      id="jenisKelaminB"
                      value="1"
                      class="mr-2 transform scale-125"
                    />
                    <label for="jenisKelaminB" class="cursor-pointer"
                      >Laki-laki</label
                    >
                  </td>
                </tr>

                <tr class="grid grid-cols-3 gap-4 mt-1 py-1">
                  <td>Status Kawin</td>
                  <td>
                    <input
                      type="radio"
                      name="statusKawin"
                      id="statusKawinA"
                      value="0"
                      class="mr-2 transform scale-125"
                    />
                    <label for="statusKawinA" class="cursor-pointer"
                      >Kawin</label
                    >
                  </td>
                  <td>
                    <input
                      type="radio"
                      name="statusKawin"
                      id="statusKawinB"
                      value="1"
                      class="mr-2 transform scale-125"
                    />
                    <label for="statusKawinB" class="cursor-pointer"
                      >Tidak Kawin</label
                    >
                  </td>
                </tr>
              </table>
            </div>

            <div
              class="flex place-items-center mt-1 bg-white px-2 transition-all border-2 border-transparent w-full font-bold"
              title="Masukkan NIK Sesuai KTP"
            >
              <input
                placeholder="Nomor Induk Kependudukan"
                type="text"
                name=""
                v-model="state.nik"
                class="pl-2 pr-3 py-2 outline-none w-full"
              />
            </div>
            <span
              v-if="v$.nik.$error"
              class="text-status-error text-left text-sm mt-1"
            >
              NIk harus berisi angka dan harus memuat 16 karakter.
            </span>

            <div
              class="flex place-items-center mt-1 bg-white px-2 transition-all border-2 border-transparent w-full font-bold"
              title="Masukkan Nama Tanpa Gelar"
            >
              <input
                placeholder="Nama Ibu Kandung"
                type="text"
                name=""
                class="pl-2 pr-3 py-2 outline-none w-full upper-case"
              />
            </div>

            <div
              class="flex place-items-center mt-1 bg-white px-2 transition-all border-2 border-transparent w-full font-bold"
              title="Diutamakan No HP Terdaftar Whatsapp"
            >
              <input
                placeholder="Nomor Handphone / WA"
                type="text"
                name=""
                v-model="state.telp"
                class="pl-2 pr-3 py-2 outline-none w-full"
              />
            </div>
            <span
              v-if="v$.telp.$error"
              class="text-status-error text-left text-sm mt-1"
            >
              Hanya bisa memuat angka
            </span>

            <div
              class="flex place-items-center mt-1 bg-white px-2 transition-all border-2 border-transparent font-bold"
              title="Masukkan E-mail Yang Aktif Anda Gunakan"
            >
              <input
                placeholder="E-mail Anda"
                type="email"
                name=""
                v-model="state.email"
                class="pl-2 pr-3 py-2 outline-none w-full"
              />
            </div>
            <span
              v-if="v$.email.$error"
              class="text-status-error text-left text-sm mt-1"
            >
              Format email tidak sesuai
            </span>

            <div
              data-v-130bfb92=""
              class="flex place-items-center mt-1 bg-white px-2 transition-all border-2 border-transparent font-bold"
              minlength="6"
              title="Buat Password Anda Minimal 6 Karakter"
            >
              <input
                data-v-130bfb92=""
                placeholder="Buat Password"
                v-model="state.password.password"
                type="password"
                class="px-3 py-2 outline-none w-full"
              />
              <div data-v-130bfb92="" class="cursor-pointer">
                <svg
                  data-v-130bfb92=""
                  aria-hidden="true"
                  focusable="false"
                  data-prefix="fa"
                  data-icon="eye"
                  role="img"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 576 512"
                  class="svg-inline--fa fa-eye fa-w-18"
                >
                  <path
                    data-v-130bfb92=""
                    fill="currentColor"
                    d="M572.52 241.4C518.29 135.59 410.93 64 288 64S57.68 135.64 3.48 241.41a32.35 32.35 0 0 0 0 29.19C57.71 376.41 165.07 448 288 448s230.32-71.64 284.52-177.41a32.35 32.35 0 0 0 0-29.19zM288 400a144 144 0 1 1 144-144 143.93 143.93 0 0 1-144 144zm0-240a95.31 95.31 0 0 0-25.31 3.79 47.85 47.85 0 0 1-66.9 66.9A95.78 95.78 0 1 0 288 160z"
                    class=""
                  ></path>
                </svg>
              </div>
            </div>
            <span
              v-if="v$.password.password.$error"
              class="text-status-error text-left text-sm mt-1"
            >
              Password Minimal terdiri 6 karakter
            </span>

            <div
              data-v-130bfb92=""
              class="flex place-items-center mt-1 bg-white px-2 transition-all border-2 border-transparent font-bold rounded-b-lg"
              minlength="6"
              title="Buat Password Anda Minimal 6 Karakter"
            >
              <input
                data-v-130bfb92=""
                placeholder="Konfirmasi Buat Password"
                v-model="state.password.confirm"
                type="password"
                class="px-3 py-2 outline-none w-full"
              />
              <div data-v-130bfb92="" class="cursor-pointer">
                <svg
                  data-v-130bfb92=""
                  aria-hidden="true"
                  focusable="false"
                  data-prefix="fa"
                  data-icon="eye"
                  role="img"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 576 512"
                  class="svg-inline--fa fa-eye fa-w-18"
                >
                  <path
                    data-v-130bfb92=""
                    fill="currentColor"
                    d="M572.52 241.4C518.29 135.59 410.93 64 288 64S57.68 135.64 3.48 241.41a32.35 32.35 0 0 0 0 29.19C57.71 376.41 165.07 448 288 448s230.32-71.64 284.52-177.41a32.35 32.35 0 0 0 0-29.19zM288 400a144 144 0 1 1 144-144 143.93 143.93 0 0 1-144 144zm0-240a95.31 95.31 0 0 0-25.31 3.79 47.85 47.85 0 0 1-66.9 66.9A95.78 95.78 0 1 0 288 160z"
                    class=""
                  ></path>
                </svg>
              </div>
            </div>
            <span
              v-if="v$.password.confirm.$error"
              class="text-status-error text-left text-sm mt-1"
            >
              Password yang anda masukkan tidak sama
            </span>

            <!-- <div class="text-status-error text-left text-sm mt-1"><div class="hints p-2 mt-2"><h2>Data Yang Belum Terisi:</h2><p> Jenis Kelamin </p><p> Status Kawin </p></div><div class="hints p-2 mt-2"><h2>Petunjuk Password</h2><p> Minimal 6 karakter. </p><p> Harus mengandung minimal satu huruf kecil. </p><p> Harus mengandung minimal satu huruf besar. </p><p> Harus mengandung minimal satu angka. </p></div></div> -->

            <div class="pt-2">
              <div class="text-left text-sm text-neutral-black font-bold">
                Silahkan jawab pertanyaan berikut
              </div>

              <div class="flex pt-3">
                <div
                  class="outline-none border-2 bg-white border-transparent transition-all rounded-md mr-2 px-2 placeholder-neutral-7"
                >
                  <span>{{ randomNumber1 }}</span
                  >+<span>{{ randomNumber2 }}</span>
                </div>

                <div class="px-4 text-base place-self-center">=</div>
                <div class="flex">
                  <input
                    placeholder="Jawaban Anda"
                    type="text"
                    title="Jawaban Anda"
                    v-model="answer"
                    class="w-full outline-none border-2 border-transparent transition-all rounded-md mr-2 px-2 placeholder-neutral-7 text-center"
                  />
                </div>
                <button @click="handleCaptcha" type="button">
                  Buat pertanyaan baru
                </button>
              </div>
            </div>
            <div class="mt-4">
              <button
                @click="submitForm"
                type="button"
                class="w-full p-2 submit-form rounded-lg text-white font-bold mb-5"
              >
                Buat Akun Mahasiswa Baru
              </button>
            </div>
          </div>
        </form>
      </div>
      <div class="image h-full hidden xl:block">
        <img
          src="./assets/bg-login.png"
          alt=""
          class="block object-cover rounded-5xl"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
@tailwind base;
@tailwind components;
@tailwind utilities;

.submit-form {
  background-color: #3389fe;
}
.upper-case {
  text-transform: uppercase;
}

.text-status-error {
  --tw-text-opacity: 1;
  color: rgba(218, 20, 20, var(--tw-text-opacity));
}
.nav-list {
  color: rgba(0, 85, 208, 0.7);
  font-size: 1.25rem;
  margin-top: 1.5rem;
  display: inline;
}
li {
  display: inline;
  margin-left: 15px;
}
.text-secondary-blue-dark {
  --tw-text-opacity: 1;
  color: rgba(0, 42, 118, var(--tw-text-opacity));
}
.text-2xl {
  font-size: 1.5rem;
  line-height: 2rem;
}
.text-primary-default {
  --tw-text-opacity: 1;
  color: rgba(0, 85, 208, var(--tw-text-opacity));
}
.text-neutral-5 {
  --tw-text-opacity: 1;
  color: rgba(133, 140, 148, var(--tw-text-opacity));
}
</style>
