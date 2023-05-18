<template>
  <div id="main-page">
    <div id="header"><Header/></div>
    <div id="topo"><SimsennetGraph/></div>
    <div id="option"><Gestion/></div>
    <div id="footer"><Footer/></div>
  </div>
</template>

<script lang="ts">
import SimsennetGraph from "@/components/SimsennetGraph.vue";
import Header from "@/components/Header.vue";
import Footer from "@/components/Footer.vue";
import Gestion from "@/components/Gestion.vue";
import {defineComponent} from "vue";

export default defineComponent({
  name: 'MainView',
  components: {
    SimsennetGraph,
    Header,
    Footer,
    Gestion
  }
});
</script>

<style lang="scss" scoped>
#header{
  grid-area: header;
  border-bottom:black 1px solid ;
    
}
#topo{
  grid-area: topo;
  padding: 1%;
  margin-left: 1%;

  
}
#option{
  border-left:black 1px solid;
  grid-area: option;
  margin-right: 1%;
  padding: 1%;

  
}
#graphic{
  display: block;
}
#footer{
  border-top: black 1px solid;
  grid-area: footer;
  padding: 1%;
  
  
}
#main-page {
  height: 90%;
  width: 100%;
  display: grid;
  row-gap: 3%;
  column-gap: 3%;
  grid-template-columns: repeat( auto-fit, minmax(250px, 1fr) );
  grid-template-rows: 10% 40% 40% 10%;
  grid-template-areas: 
    "header header header"
    "topo topo option"
    "topo topo option"
    "footer footer footer";
}

</style>

