<template>
  <div id="app">
    <div>
      <button @click="checkAll()">Tich all</button>
      <input type="text" v-model="textTask" @keypress.enter="addTask()">
    </div>

    <div v-show="show" v-for="(task, index) in tasksChange" :key="index">
      <input type="checkbox" v-model="task.done" @click="clickTaskToDo(task, index)">
      <label type="text" :class="{done: task.done}" @dblclick="changeTextTask(task)">{{ task.content }} </label>
      <button @click="Delete(index)">Delete</button>
    </div>
    <div>
      {{ countToDo }} items left
      <button @click="Active('All')">All</button>
      <button @click="Active('active')">Active</button>
      <button @click="Active('completed')">Completed</button>
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
      tasksChange: [
        {content: 'di cho', done: false},
        {content: 'nau an', done: false},
        {content: 'mua sam', done: false},
        {content: 'chay the duc', done: false},
        {content: 'xem phim', done: false},
      ],
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
    clickTaskToDo: function (task, index) {
      task.done = !task.done
      this.tasks[index].done = !this.tasks[index].done
      this.loadCountToDo();
    },

    checkAll: function () {
      let doneTask = false;
      for (let i = 1; i < this.tasks.length; i++) {
        if (this.tasks[i - 1].done != this.tasks[i].done) {
          doneTask = true;
          break;
        }
      }

      if (doneTask == true) {
        this.tasks.forEach((task) => {
          if (!task.done) {
            task.done = true
          }
        })
        this.tasksChange.forEach((task) => {
          if (!task.done) {
            task.done = true
          }
        })
      } else {
        this.tasks.forEach(task => task.done = !task.done)
        this.tasksChange.forEach(task => task.done = !task.done)
      }
      this.loadCountToDo();
    },

    addTask: function () {
      this.tasks.push({content: this.textTask, done: false})
      this.tasksChange.push({content: this.textTask, done: false})
      this.textTask = ''
      this.loadCountToDo()
    },

    Delete: function (index) {
      this.tasks.splice(index, 1);
      this.tasksChange.splice(index, 1);
      this.loadCountToDo();
    },

    Active: function (status) {
      this.tasksChange.length = 0;
      console.log(JSON.stringify(this.tasks));
      if (status == 'active') {
        this.tasks.forEach((task) => {
          if (!task.done) {
            this.tasksChange.push(task)
          }
        })
      } else if (status == 'completed') {
        this.tasks.forEach((task) => {
          if (task.done) {
            this.tasksChange.push(task)
          }
        })
      } else {
        this.tasks.forEach((task) => {
          this.tasksChange.push(task)
        })
      }
    },
    ClearTaskCompleted: function () {
      this.tasks.forEach((item, index) => {
        if (item.done) {
          // newTasks.push(item)
          this.tasks.splice(index,1);
        }
      })
      console.log(JSON.stringify(this.tasks));
      return this.tasksChange = [...this.tasks];
    },

    changeTextTask: function (oldTask) {
      oldTask.content = 'abcdefgh'
    },
  }
}

</script>

<style>
.done {
  text-decoration: line-through;
}
</style>
