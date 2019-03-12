// Sample.vue

<template>
   <div class="box" :class="{highlight: selected}">
      <div class="sample" :style="{'background': color.rgba}"></div>
      <div class="label" >{{label}}</div>
  </div>
</template>
<style scoped>
.sample{
  width: 80px;
  height: 80px;
}
.label{
  font-size: 10px;
  display: block;
  padding-left: 4px;
}
.highlight{
  border-bottom: 5px solid #a3a3a3;
}
</style>

<script>

   export default {
     props:['color'],
   data() {
         return {
            label: '',
            selected: false,
         };
      },
   methods: {
   }
  }
</script>