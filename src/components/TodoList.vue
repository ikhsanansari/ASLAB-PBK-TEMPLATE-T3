<script setup>
import { ref, computed, onMounted, watch } from 'vue'

// Referensi dan reaktivitas
const inputNamaMahasiswa = ref(null)
const namaBaru = ref('')
const alamatBaru = ref('')
const daftarMahasiswa = ref([])

// Computed Property: menghitung total mahasiswa
const totalMahasiswa = computed(() => daftarMahasiswa.value.length)

// Fungsi untuk menambahkan mahasiswa
const tambahMahasiswa = () => {
  if (!namaBaru.value.trim()) return // Cegah input kosong
  daftarMahasiswa.value.push({
    id: Date.now(),
    nama: namaBaru.value,
    alamat: alamatBaru.value.trim() || 'Alamat tidak diinputkan', // default
  })
  // Reset input setelah menambah data
  namaBaru.value = ''
  alamatBaru.value = ''
  inputNamaMahasiswa.value.focus()
}

// Fungsi untuk menghapus mahasiswa berdasarkan id
const hapusMahasiswa = (id) => {
  daftarMahasiswa.value = daftarMahasiswa.value.filter(m => m.id !== id)
}

// Lifecycle: fokus ke input nama saat komponen dimuat
onMounted(() => {
  inputNamaMahasiswa.value.focus()
})

// Watcher: memantau perubahan nilai nama mahasiswa
watch(namaBaru, (nilaiBaru, nilaiLama) => {
  console.log(`Nama berubah dari "${nilaiLama}" ke "${nilaiBaru}"`)
})
</script>

<template>
  <div class="form-wrapper">
    <h2>Todo-List Absensi</h2>
    <input
      v-model="namaBaru"
      ref="inputNamaMahasiswa"
      placeholder="Nama mahasiswa"
      class="form-input"
    />
    <input
      v-model="alamatBaru"
      placeholder="Alamat mahasiswa"
      class="form-input"
    />
    <button @click="tambahMahasiswa" class="form-button">Daftar</button>
    
    <!-- Menampilkan daftar mahasiswa -->
    <ol>
      <li v-for="mahasiswa in daftarMahasiswa" :key="mahasiswa.id" class="student-item">
        {{ mahasiswa.nama }} - {{ mahasiswa.alamat }}
        <button @click="hapusMahasiswa(mahasiswa.id)" class="delete-button">Hapus</button>
      </li>
    </ol>

    <p>Total mahasiswa: {{ totalMahasiswa }}</p>
  </div>
</template>

<style scoped>
.form-wrapper {
  max-width: 400px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
  background: #fff;
  padding: 0;
  text-align: center;
}
h2 {
  margin-bottom: 20px;
  color: #333;
}
.form-input {
  width: 100%;
  padding: 8px 10px;
  margin-bottom: 12px;
  border: 1px solid black;
  box-sizing: border-box;
}
.form-button {
  width: 100%;
  padding: 10px 0;
  background: #4caf50;
  color: white;
  border: none;
  cursor: pointer;
  font-weight: bold;
}
.form-button:hover {
  background: #45a049;
}
ol {
  padding-left: 40px;
  color: #555;
  font-size: 16px;
  text-align: left;
}
.student-item {
  margin-bottom: 8px;
}
.delete-button {
  margin-left: 10px;
  padding: 3px 8px;
  background: #e74c3c;
  border: none;
  color: white;
  cursor: pointer;
}
.delete-button:hover {
  background: #c0392b;
}
p {
  margin-top: 20px;
  color: #333;
  font-weight: 600;
}
</style>
