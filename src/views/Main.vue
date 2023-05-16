<template>
  <div id="main-page">
    <div id="header"><Header></Header></div>
    <div id="topo"><SimsennetGraph></SimsennetGraph></div>
    <div id="option"><Gestion></Gestion></div>
    <div id="footer"><Footer></Footer></div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import SimsennetGraph  from "@/components/SimsennetGraph.vue";
import Header from "@/components/Header.vue";
import Footer from "@/components/Footer.vue";
import Gestion from "@/components/Gestion.vue";

@Options({
  components: {
    SimsennetGraph,
    Header,
    Footer,
    Gestion,
  },
})
export default class Main extends Vue {}
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

