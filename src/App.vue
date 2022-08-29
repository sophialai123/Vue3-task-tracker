<template>

<div class="container">
  <!-- pass the props here -->
<TaskHeader @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask"/>
<div v-if="showAddTask" >
<!-- call emit from addtask children -->
<AddTask  @add-task="addTask"/>
</div>

<!-- v:bind the tasks data,add methods here
delete-task is from two level down Task.vue component -->
<TasksArray 
@toggle-reminder="toggleReminder"
@delete-task="deleteTask" :tasks="tasks"/>
</div>

</template>




<script>
import TaskHeader from "./components/Header.vue";
import TasksArray from './components/Tasks.vue';
import AddTask from './components/AddTask.vue'

export default {
  name: 'App',
  components: {
    TaskHeader,
    TasksArray,
    AddTask,
},
data(){
  return {
    tasks:[],
    showAddTask:false
  }
},
methods:{
  //fetch data from db.json 
  async fetchTasks(){
    const res = await fetch('http://localhost:3000/tasks')

    const data = await res.json()
    return data
  },
  //fetch a single task  
  async fetchTask(id){
    const res = await fetch(`http://localhost:3000/tasks/${id}`)

    const data = await res.json()
    return data
  },
  toggleAddTask(){
    this.showAddTask=!this.showAddTask
  },


  
  //add a new task
  async addTask(task){
  //make a request POST resquest 
    const res = await fetch('http://localhost:3000/tasks',
    {method:"POST",
      headers:{
        'Content-type':'application/json'
      },
      body:JSON.stringify(task)
    })
     //get new task back from serve
     const data = await res.json()
    //set this.tasks equal to copy exsit tasks
    // and puls new task
    this.tasks = [...this.tasks,data]

  },


//make a request DELETE resquest 
  async deleteTask(id){
    if(confirm("Are you sure?")){
      const res = await fetch(`http://localhost:3000/tasks/${id}`,{
      method:'DELETE'
    })
    
    //check the status,
    //if status is 200, delete task id
    //otherwise alert
    res.status === 200 ? (this.tasks = this.tasks.filter((task)=> task.id !== id)): alert("Error deleting task")
    }
    
  },

  //toggle task reminder the border color 
  async toggleReminder(id){
    
    //toggle the single task
    const taskToToggle = await this.fetchTask(id)
    //set to opposit the reminder boolean
    const updTask = {...taskToToggle,reminder:!taskToToggle.reminder}


    //make a UPDATE request
    const res =  await fetch(`http://localhost:3000/tasks/${id}`,{
      method: "PUT",
      headers:{
        'Content-type':'application/json'
      },
      body: JSON.stringify(updTask)
    })

    const data = await res.json()
   //updated the tasks
    this.tasks = this.tasks.map((task)=>
    //if the task.id match id, the copy the task object
    //and set the opposite of task reminder,otherwise just task
    task.id ===id? {...task,reminder: data.reminder}: task
    )
  }
},
//promise fetch data from serve
async created(){
  //get data from fetch data from json
  this.tasks= await this.fetchTasks()
},
}
</script>

<!-- Gloabl -->
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
