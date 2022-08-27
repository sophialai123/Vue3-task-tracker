<template>
<!-- v-for: loop through the tasks array -->
<div v-for="task in tasks" :key="task.id" >
<!-- pass the componet here and props data , $emit one level up 
which is  app component  -->
<EachTask 
 @toggle-reminder="$emit('toggle-reminder',task.id)"
 @delete-task="$emit('delete-task',task.id)" :task='task'/>
</div>
</template>

<script>
import EachTask from './Task.vue';
export default {
  name:"TasksArray",
  //props type
  props:{
    tasks: Array
  },
  //pass the child task component
  components:{
    EachTask
  },

  emits:['delete-task','toggle-reminder']

}
</script>