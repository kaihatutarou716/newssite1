<template>
  <div id="container">
    <app-pageContents></app-pageContents>
  </div>
</template>
 
<script>
import pageContents from "./components/pageAll.vue";

export default {
  name: "app",
  components: {
    "app-pageContents": pageContents
  }
};
</script>
<style scoped>
#container {
  background-color: #f6f7f9;
  margin: 0px auto;
  padding: 20px;
  border-radius: 20px;
  width: 1500px;
  height: 1500px;
}

body {
  background-color: #f6f7f9;
}
</style>