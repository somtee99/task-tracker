<template>
    <AddTask v-show="showAddTask" @add-task="addTask" /> 
    <Tasks @delete-task="deleteTask" 
    @toogle-reminder="toogleReminder" :tasks="tasks"/>
</template>

<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';

export default {
    name: 'Home',
    components: {
        Tasks, AddTask
    },
    props: {
        showAddTask: Boolean,
    },
    data(){
        return {
          tasks: [],  
        }  
    },
    methods: {
        async addTask(task){
            const res = await fetch('api/tasks', {
                method: 'POST',
                headers: {
                'Content-type': 'application/json',
                },
                body: JSON.stringify(task)
            })

            const data = await res.json();
            console.log(data);
            this.tasks = [...this.tasks, data];
        },
    async deleteTask(id){
      if(confirm('Are you sure you want to delete this task?')){
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE'
        })
        res.status === 200 ? (
          this.tasks = this.tasks.filter(
            (task) => task.id !== id))
          : alert('Error deleting task')       
      }
    },
    async toogleReminder(id){
      const toggledTask = await this.fetchTask(id)
      const updatedTask = {...toggledTask, reminder: !toggledTask.reminder}

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updatedTask)
      })

      const data = await res.json()

      if(res.status == 200){
        this.tasks = this.tasks.map((task) =>
          task.id === id? 
            {...task, reminder: !task.reminder}
            : task
        )
      }
    },
    async fetchTasks(){
      const res = await fetch('api/tasks');
      const data = await res.json();

      return data;
    },
    async fetchTask(id){
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();

      return data;
    }
    },
    async created(){
        this.tasks = await this.fetchTasks();
    }
}
</script>