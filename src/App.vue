<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="addTask">
      <input v-model="newTask" placeholder="Tambahkan kegiatan baru" />
      <button type="submit">Tambah</button>
    </form>

    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        {{ task.text }}
        <button @click="removeTask(index)">Batal</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        { text: "Belajar ", completed: false },
        { text: "Makan", completed: false }
      ]
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({ text: this.newTask, completed: false });
        this.newTask = "";
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    }
  }
};
</script>

<style>
.container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
}
input[type="text"] {
  width: 70%;
  padding: 8px;
}
button {
  margin-left: 10px;
}
</style>
