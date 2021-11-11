<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask" />
    <AddTask v-show="showAddTask" @add-task="addTask" /> 
    <Tasks @delete-task="deleteTask" 
    @toogle-reminder="toogleReminder" :tasks="tasks"/>
  </div>
</template>

<script>
import Header from './components/Header';
import Tasks from './components/Tasks';
import AddTask from './components/AddTask';

export default {
  name: 'App',
  components: {
    Header, Tasks, AddTask
  },
  data(){
    return {
      tasks: [],
      showAddTask: false
    }
  },
  methods: {
    toggleAddTask(){
      this.showAddTask = !this.showAddTask
    },
    addTask(task){
      this.tasks = [...this.tasks, task];
    },
    deleteTask(id){
      if(confirm('Are you sure you want to delete this task?')){
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    toogleReminder(id){
      this.tasks = this.tasks.map((task) =>
        task.id === id? 
          {...task, reminder: !task.reminder}
          : task
      )
    },
  },
  created(){
    this.tasks = [
      {
        id: 1,
        text: 'Dentist Appointment',
        day: 'September 20 at 12:00pm',
        reminder: true
      },
      {
        id: 2,
        text: 'Drop Kids at School',
        day: 'September 24 at 9:00am',
        reminder: true
      },
      {
        id: 3,
        text: 'Guys Night Out',
        day: 'September 30 at 10:00pm',
        reminder: false
      }
    ];
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
