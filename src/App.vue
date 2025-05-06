<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="addTask">
      <input v-model="newTask" placeholder="Tambahkan kegiatan baru" />
      <button type="submit">Tambah</button>
    </form>

    <label>
      <input type="checkbox" v-model="showOnlyUnfinished" />
      Tampilkan hanya kegiatan yang belum selesai
    </label>

    <ul>
      <li v-for="(task, index) in filteredTasks" :key="index">
        <input type="checkbox" v-model="task.completed" @change="handleCheck(task)" />
        <span :class="{ done: task.completed }">{{ task.text }}</span>
        <button @click="removeTask(index)">Batal</button>
        <div v-if="task.showMessage" class="message">
          Kegiatan ini sudah selesai
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      showOnlyUnfinished: false,
      tasks: [
        { text: "Belajar VueJS", completed: false, showMessage: false },
        { text: "Mengerjakan tugas PBK", completed: false, showMessage: false }
      ]
    };
  },
  computed: {
    filteredTasks() {
      return this.showOnlyUnfinished
        ? this.tasks.filter(task => !task.completed)
        : this.tasks;
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({ text: this.newTask, completed: false, showMessage: false });
        this.newTask = "";
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    handleCheck(task) {
      if (task.completed) {
        task.showMessage = true;
        setTimeout(() => {
          task.showMessage = false;
        }, 5000);
      }
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
.done {
  text-decoration: line-through;
  color: gray;
}
.message {
  color: green;
  font-size: 0.9em;
  margin-top: 5px;
}
input[type="text"] {
  width: 70%;
  padding: 8px;
}
button {
  margin-left: 10px;
}
</style>
