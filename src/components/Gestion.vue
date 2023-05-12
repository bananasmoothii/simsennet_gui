<script lang="js">
import Button from "@/components/Buttons/Button.vue"
import VueNumberInput from "@/components/vue-number-input.vue"
export default {
  name: 'Gestion-component',
  data(){
    return {
        label_button: "Pause",
        isRunning: true,
        value: "",
    }
  },
  methods: {
        test: function(){
          return 0
        },
        button_action: function(){
            if(this.isRunning){
                /*appel async pause*/
                this.isRunning=false;
                this.label_button="Reprendre";
            }
            else{
                /*appel async run*/
                this.isRunning=true;
                this.label_button="Pause";
            }
        },
        onUpdate: function(newValue, oldValue) {
          if(isNaN(newValue))
          {
            this.value=oldValue
          }
        }
  },
  components:{
    VueNumberInput,
    Button
  },
}

</script>


<template>
<div id="Gestion-container">
  <div class="title">Gestion du réseau :</div>
  <div id="buttons">
    <Button :color="'#8dba8a'" :func="this.test" :text="'Télécharger'"></Button>
    <Button :color="'#8d90eb'" :func="button_action" :text="this.label_button"></Button>
    <Button :color="'#cf6d71'" :func="this.test" :text="'Arrêter'"></Button>
  </div>
  <hr/>
  <div class="title">Paramètres du réseau :</div>
  <div id="container-freq">
    <div id="freq-input" >
    <vue-number-input id="vue-number-input" v-for="n in 10" :key="n" placeholder="Fréquence" v-model="value" :min="1" :max="Infinity" :size="large" :rounded="true" inline controls @update:model-value="onUpdate">
  </vue-number-input>
</div>
</div>
</div>

</template>

<style lang="scss" scoped>
    #container-freq{
      height: 300px;
      width: auto;
      overflow-y: auto;  
    }

    #freq-input{
      display: flex;
      flex-direction: column;
      gap: 1rem;  
      justify-content: center;
      align-items: center;
      width: 100%;
    }

    hr{
      margin-top: 50px;
      margin-left: 15px;
      margin-right: 15px;
      margin-bottom: 50px;
      height: 0.5px;
      border: none;
      background-color: black;
    }

    #buttons{
        display: flex;
        flex-direction: column;
        gap: 1rem;
        align-items: center;
        justify-content: center;
        
    }
    .title{
        display: block;
        font-size: 1.5vw;
        justify-content: center;
        text-align: center;
        padding-bottom: 50px;
    }
</style>