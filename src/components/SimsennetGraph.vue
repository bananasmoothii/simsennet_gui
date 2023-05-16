<script lang="js">
import { VNetworkGraph } from "v-network-graph"
import "v-network-graph/lib/style.css"
import {computed, reactive, ref, watch} from "vue"
import * as vNG from "v-network-graph"
import {ForceLayout,ForceNodeDatum,ForceEdgeDatum} from "v-network-graph/lib/force-layout"
import { EventHandlers } from "v-network-graph"
import ModalGraph from "@/components/ModalGraph.vue"


const zoomLevel = ref(1.5)

  const configs = reactive(
    vNG.defineConfigs({
      view: {
        layoutHandler: new ForceLayout({positionFixedByDrag: false, positionFixedByClickWithAltKey: true}),
        scalingObjects: true,
        minZoomLevel: 0.1,
        maxZoomLevel: 16,
      },
    })
)

export default {
  name: 'SimsennetGraph',
  components: {
    VNetworkGraph,
    ModalGraph,

  },
  provide(){
    return {
      provideDisplayGraph: this.displayGraph
    }
  },
  props:{
    graph: String
  },
  mounted () {
    this.get_node();
    this.timer = setInterval(this.get_node, 30000);
  },
  beforeUnmount() {
    clearInterval(this.timer);
  },
  data(){
    return {
      data: reactive({}),
      config: configs,
      zoomLevel: zoomLevel,
      timer: '',
      isValid: false,
      node_name: '',
      displayGraph: {value: false},
      url: 'http://ssngwtd.loria.fr/network-map.php',
      d3ForceEnabled: computed({
        get: () => configs.view.layoutHandler instanceof ForceLayout,
        set: (value) => {
          if (value) {
            configs.view.layoutHandler = new ForceLayout()
          } else {
            configs.view.layoutHandler = new vNG.SimpleLayout()
          }
        },
      }),
      click_event: {
        'node:click': ({ node }) => {
          window.console.log(this.data["nodes"][node]["name"]);
          this.node_name = this.data["nodes"][node]["name"];
          this.displayGraph.value = true;
        },
      }
    }
  },
  methods: {
    get_node: function() {
      let request = new XMLHttpRequest();
      request.open("GET", this.url, false);
      request.send();
      try {
        let jsonResult = JSON.parse(request.responseText);
        this.data["nodes"] = jsonResult["nodes"];
        this.data["edges"] = jsonResult["edges"]; 
        this.isValid = true;
      } catch (error) {
        this.isValid = false;
      }
      
      
    },
  }
}

</script>

<template>
  <div v-if="isValid" id="graph_part">
  <div class="demo-control-panel">
    <ModalGraph :node_name="this.node_name"></ModalGraph>
    <label for="scaleObj">Scaling Objects:</label><input type="checkbox" v-model="config.view.scalingObjects" name="scaleObj"> |
    <label for="forceEnabled">Automatic Node Positioning:</label><input type="checkbox" v-model="d3ForceEnabled" name="forceEnabled">
  </div>
  <v-network-graph class="graph" :event-handlers="click_event" :nodes="data['nodes']" :edges="data['edges']" :layouts="data['layouts']" v-model:zoom-level="zoomLevel" :configs="config"></v-network-graph>
  </div>
  <div v-else id="error_occured">Connectez au moins un capteur !</div>
</template>

<style scoped>
#graph_part{
  height: 100%;
}
.graph {
  width: 100%;
  height: 100%;
}
#error_occured{
  padding-top: 50px;
  color: red;
  text-align: center;
  font-size: 1.5em;
}
</style>