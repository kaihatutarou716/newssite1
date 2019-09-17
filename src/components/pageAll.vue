<template>
  <div>
    <app-header></app-header>
    <div id="newQiiaP">
      <app-newQiitaartles></app-newQiitaartles>
    </div>
    <div id="sideP">
      <app-sideContents></app-sideContents>
    </div>
    <div id="itemConts">
      <app-itemContents></app-itemContents>
    </div>
  </div>

</template>
 
<script>
import Header from "./Header.vue";
import newQiitaartles from "./newQiitaartles.vue";
import sideContents from "./sideContents.vue";
import itemContents from "./itemContents.vue";

export default {
  components: {
    "app-header": Header,
    "app-newQiitaartles": newQiitaartles,
    "app-sideContents": sideContents,
    "app-itemContents": itemContents
  }
};
</script>
<style scoped>
#newQiiaP {
  width: 900px;
  padding-top: 30px;
  padding-bottom: 50px;
  float: left;
}

#sideP {
  width: 500px;
  padding-top: 30px;
  padding-bottom: 50px;
  float: right;
}

#itemConts {
  width: 900px;
  padding-bottom: 50px;
  float: left;
}
</style>