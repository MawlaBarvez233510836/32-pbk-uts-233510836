<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        {{ task.text }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [
        { text: "Belajar ", completed: false },
        { text: "makan", completed: false }
      ]
    };
  }
};
</script>

<style>
.container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
}
</style>
