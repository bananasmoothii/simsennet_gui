<script lang="js">
import { VNetworkGraph } from "v-network-graph"
import "v-network-graph/lib/style.css"
import {computed, reactive, ref, watch} from "vue"
import * as vNG from "v-network-graph"
import {ForceLayout,ForceNodeDatum,ForceEdgeDatum} from "v-network-graph/lib/force-layout"
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
    VNetworkGraph
  },
  props:{
    graph: String
  },
  mounted () {
    this.data["nodes"] = {
        node1 : { name: "Node 1"},
        node2 : { name: "Node 2"},
        node3 : { name: "Node 3"},
        node4 : { name: "Node 4"}
      }
      this.data["edges"] = {
        edge1: { source: "node1", target: "node2"},
        edge2: { source: "node2", target: "node3" },
        edge3: { source: "node3", target: "node4" }
      }
      this.data["layouts"] = {
        nodes: {
          node1: {
            x: 0,
            y: 0,
            fixed: true,
          },
        }
      }
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
      })
    }
  },
  methods: {
    get_node() {
      /*let request = new XMLHttpRequest();
      request.open("GET", this.url, false);
      request.onload = function () {
        console.log(this.data)
      };
      request.send();
      let jsonResult = JSON.parse(request.responseText);
      this.data["nodes"] = reactive(jsonResult["nodes"])
      this.data["edges"] = reactive(jsonResult["edges"])
      this.data["layouts"] = reactive(jsonResult["layouts"])
      */
      this.data["nodes"] = {
        node1 : { name: "Node 1"},
        node2 : { name: "Node 2"},
        node3 : { name: "Node 3"},
        node4 : { name: "Node 4"}
      }
      this.data["edges"] = {
        edge1: { source: "node1", target: "node2"},
        edge2: { source: "node2", target: "node3" },
        edge3: { source: "node3", target: "node4" }
      }
      this.data["layouts"]["nodes"]["node1"]["fixed"] = true;
    }
  }
}
</script>

<template>
  
  <div class="demo-control-panel">
    <label for="scaleObj">Scaling Objects:</label><input type="checkbox" v-model="config.view.scalingObjects" name="scaleObj"> |
    <label for="forceEnabled">Automatic Node Positioning:</label><input type="checkbox" v-model="d3ForceEnabled" name="forceEnabled">
  </div>
  <v-network-graph class="graph" :nodes="data['nodes']" :edges="data['edges']" :layouts="data['layouts']" v-model:zoom-level="zoomLevel" :configs="config"></v-network-graph>
</template>

<style scoped>
.graph {
  width: 100%;
  height: 90%;
}
</style>