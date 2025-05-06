<template>
  <div class="container">
    <h1><span class="header">Daftar Kegiatan</span></h1>

    <form @submit.prevent="addTask" class="form">
      <input v-model="newTask" placeholder="Tambahkan kegiatan baru" />
      <button type="submit">Tambah</button>
    </form>

    <button class="filter-btn" @click="toggleFilter">
      {{ showOnlyUnfinished ? 'Tampilkan Semua Kegiatan' : 'Tampilkan Belum Selesai' }}
    </button>

    <ul>
      <li v-for="(task, index) in filteredTasks" :key="index" class="task-item">
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
      tasks: []
    };
  },
  computed: {
    filteredTasks() {
      return this.showOnlyUnfinished
        ? this.tasks.filter(task => !task.completed)
        : this.tasks;
    }
  },
  mounted() {
    const savedTasks = localStorage.getItem("tasks");
    if (savedTasks) {
      this.tasks = JSON.parse(savedTasks);
    }
  },
  watch: {
    tasks: {
      handler(newTasks) {
        localStorage.setItem("tasks", JSON.stringify(newTasks));
      },
      deep: true
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
    },
    toggleFilter() {
      this.showOnlyUnfinished = !this.showOnlyUnfinished;
    }
  }
};
</script>

<style>
body {
  background-image: url('https://wallpapercave.com/wp/wp4575212.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
  color: gold;
}

.container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
  background-color: rgba(40, 42, 43, 0.85);
  border-radius: 12px;
  box-shadow: 0 0 15px rgba(255, 255, 255, 0.5);
  margin-top: 40px;
}

.header {
  color: rgb(249, 245, 245);
}

input[type="text"] {
  width: 70%;
  padding: 8px;
  color: gold;
  background: #333;
  border: 1px solid gold;
}

button {
  margin-left: 10px;
  padding: 6px 12px;
  background-color: #444;
  color: gold;
  border: 1px solid gold;
  cursor: pointer;
}
button:hover {
  background-color: #666;
}

.task-item {
  margin: 10px 0;
  color: rgb(255, 255, 255);
}

.done {
  text-decoration: line-through;
  color: gray;
}

.message {
  color: rgb(144, 57, 57);
  font-size: 0.9em;
  margin-top: 5px;
}

.filter-btn {
  margin-top: 10px;
  background-color: #222;
}
</style>
