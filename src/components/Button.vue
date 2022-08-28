<template>
<!-- pass the props and listening events  -->
  <button @click="onClick()" :style="{background: color}" class="btn">
    {{text}}
  </button>
</template>





<script>
export default {
  name:'AddButton',
  props:{
    text: String,
    color: String
  },
  methods:{
    onClick(){
      this.$emit("toggle-add-task")
    }
  }
}
</script>
