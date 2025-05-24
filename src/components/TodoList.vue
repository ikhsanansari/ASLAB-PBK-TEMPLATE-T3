<script setup>
import { ref, computed, onMounted, watch } from 'vue'
const studentNameInput = ref(null)
const newStudentName = ref('')
const newStudentAlamat = ref('')
const students = ref([])

// Computed Property
const totalStudents = computed(() => students.value.length)
const addStudent = () => {
  if (!newStudentName.value.trim()) return // Cegah input kosong
  students.value.push({
    id: Date.now(), 
    name: newStudentName.value,
    alamat: newStudentAlamat.value.trim() || 'Alamat tidak diinputkan', //default
  })
  // Reset input setelah menambah data
  newStudentName.value = ''
  newStudentAlamat.value = ''
  studentNameInput.value.focus()
}
// Fungsi untuk menghapus mahasiswa berdasarkan id
const removeStudent = (id) => {
  students.value = students.value.filter(s => s.id !== id)
}
// Lifecycle Hook onMounted: Fokuskan input nama saat komponen sudah dimuat
onMounted(() => {
  studentNameInput.value.focus()
})
// Watcher: Memantau perubahan nilai input nama mahasiswa
watch(newStudentName, (newVal, oldVal) => {
  console.log(`Nama berubah dari "${oldVal}" ke "${newVal}"`)
})
</script>

<template>
  <div class="form-wrapper">
    <h2>Todo-List Absensi</h2>
    <input
      v-model="newStudentName"
      ref="studentNameInput"
      placeholder="Nama mahasiswa"
      class="form-input"
    />
    <input
      v-model="newStudentAlamat"
      placeholder="Alamat mahasiswa"
      class="form-input"
    />
    <button @click="addStudent" class="form-button">Daftar</button>
    <!-- List Rendering -->
    <ol>
      <li v-for="student in students" :key="student.id" class="student-item">
        {{ student.name }} - {{ student.alamat }}
        <!-- Tombol hapus -->
        <button @click="removeStudent(student.id)" class="delete-button">Hapus</button>
      </li>
    </ol>
    <p>Total mahasiswa: {{ totalStudents }}</p>
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