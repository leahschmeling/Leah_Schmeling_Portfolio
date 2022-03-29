<template>
  <div id="App">
    <h1 id="app-title">{{ title }}</h1>
    <span
      >You have {{ allTasks }} {{ allTasks > 1 ? "tasks" : "task" }} at the
      moment</span
    >

    <!-- Handle user input -->
    <div class="input-group">
      <input type="text" v-model="newTask" placeholder="Add a new task" />
      <button v-on:click="addTask" :disabled="newTask.length < 1">
        Add task
      </button>
    </div>

    <div class="preview" v-if="newTask.length > 0">
      <h3>New task preview</h3>
      <p>{{ newTask }}</p>
    </div>

    <div class="list">
      <p id="todo-list">List Rendering</p>
      <ul>
        <li
          v-for="(task, index) in latest"
          :key="task.id"
          @click="finishTask(task)"
          :class="{ strikeout: task.finished }"
        >
          {{ index + 1 }}. {{ task.name }}

          <div v-if="task.finished">
            <button @click="removeTask(task.id)">Delete task</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script >
export default {
  // state / instance
  data() {
    return {
      title: "My To Do App",
      newTask: "",
      tasks: [
        { id: 1, name: "Learn Vue JS", finished: false },
        { id: 2, name: "Build a Vue application", finished: false },
        { id: 3, name: "Write an article about Vue JS", finished: false },
      ],
    };
  },

  // actions
  methods: {
    addTask() {
      if (this.newTask.length < 1) return;

      this.tasks.push({
        id: this.tasks.length + 1,
        name: this.newTask,
        finished: false,
      });

      this.newTask = "";
    },
    removeTask(taskID) {
      this.tasks = this.tasks.filter((task) => {
        return task.id !== taskID;
      });
    },
    finishTask(task) {
      task.finished = !task.finished;
    },
  },

  computed: {
    allTasks() {
      return this.tasks.length;
    },
    latest() {
      return [...this.tasks].reverse();
    },
  },
};
</script>

<style scoped>
#App {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 20px;
}
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}
li:hover {
  background-color: yellow;
}
.strikeout {
  text-decoration: line-through;
}
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
