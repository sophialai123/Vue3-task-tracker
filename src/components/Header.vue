<template>

  <header>
    <h1>{{title}}</h1>
    <!-- v:bind the text and color depends on showAddTask boolean -->
    <AddButton @toggle-add-task="$emit('toggle-add-task')" 
    :text="showAddTask? 'Close Task': 'Add Task'" 
    :color="showAddTask? 'red':'green'" />

  </header>

</template>


<script>
import AddButton from './Button.vue';
export default {
  name:'TaskHeader',
  //props can be an array or object
  props:{
    //type
    title:String,
    showAddTask:Boolean
  },

  //render components here
  components:{
    AddButton,

}
}
</script>


<style scoped>
header{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;

}


</style>
