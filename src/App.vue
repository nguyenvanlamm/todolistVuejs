<template>
  <div id="app">
    <div>
      <button @click="checkAll()">Tich all</button>
      <input type="text" v-model="textTask" @keypress.enter="addTask()">
    </div>

    <div v-show="show" v-for="(task, index) in tasks" :key="index">
      <input type="checkbox" v-model="task.done" @click="clickTaskToDo(task)">
      <span :class="{done: task.done}">{{ task.content }}</span>
      <button @click="Delete(index)">Delete</button>
    </div>
    <div>
      {{ countToDo }} items left
      <button>All</button>
      <button @click="Active()">Active</button>
      <button>Completed</button>
      <button @click="ClearTaskCompleted()">Clear Completed</button>
    </div>
  </div>
</template>

<script>

export default {

  data() {
    return {
      currentToDo: null,
      show: true,
      countToDo: 5,
      textTask: '',
      tasks: [
        {content: 'di cho', done: false},
        {content: 'nau an', done: false},
        {content: 'mua sam', done: false},
        {content: 'chay the duc', done: false},
        {content: 'xem phim', done: false},
      ],
    }
  },

  methods: {
    loadCountToDo: function () {
      let count = 0;
      for (let i = 0; i < this.tasks.length; i++) {
        if (!this.tasks[i].done) {
          count++
        }
      }
      this.countToDo = count;

    },
    clickTaskToDo: function (task) {
      task.done = !task.done;
      this.loadCountToDo();
    },

    checkAll: function () {
      let tmp = false;
      for (let i = 1; i < this.tasks.length; i++) {
        if (this.tasks[i - 1].done != this.tasks[i].done) {
          tmp = true;
          break;
        }
      }

      if (tmp == true) {
        this.tasks.forEach((task) => {
          if (!task.done) {
            task.done = true
          }
        })
      } else {
        this.tasks.forEach(task => task.done = !task.done)
      }
      this.loadCountToDo();
    },

    addTask: function () {
      this.tasks.push({content: this.textTask, done: false})
      this.textTask = ''
      this.loadCountToDo()
    },

    Delete: function (index) {
      this.tasks.splice(index, 1);
      this.loadCountToDo();
    },

    Active: function () {

    },
    ClearTaskCompleted: function () {
      let newTasks = [];
      this.tasks.forEach(item => {
        if (!item.done) {
          newTasks.push(item)
        }
      })
      return this.tasks = newTasks;
    }
  }
}

</script>

<style>
.done {
  text-decoration: line-through;
}
</style>
