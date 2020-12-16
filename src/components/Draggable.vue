<template>
  <draggable v-model="myArray" group="people" @start="drag=true" @end="drag=false">
     <div v-for="element in myArray" :key="element.id">{{element.name}}</div>
  </draggable>
</template>

<script>
export default {

}
</script>
