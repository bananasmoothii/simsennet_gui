<script lang="js">
import Button from "@/components/Buttons/Button.vue"
import VueNumberInput from "@/components/vue-number-input.vue"
import ModalPlanning from "@/components/ModalPlanning.vue"

export default {
  name: 'Gestion-component',
  provide(){
    return {
      provideDisplayPlanning: this.displayPlanning,
    }
  },
  data(){
    return {
        global_freq: 'http://ssngwtd.loria.fr/get-global-freq.php',
        download_results: 'http://ssngwtd.loria.fr/download.php',
        reset: 'http://ssngwtd.loria.fr/reset.php',
        isRunning: true,
        value: "",
        freq: "",
        displayPlanning: {value: false},
    }
  },
  methods: {
        download: function(){
          window.open(this.download_results, '_self');
          
        },
        reset_func: function(){
          let request = new XMLHttpRequest();
          request.open("GET", this.reset, false);
          request.send();
        },
        button_action: function(){
            this.isRunning=!this.isRunning;
        },
        edit: function(){
            this.displayPlanning.value = true;
        },
        onUpdate: function(newValue, oldValue) {
          if(isNaN(newValue))
          {
            this.value=oldValue;
          }
        },
        get_global_freq: function(){
          let request = new XMLHttpRequest();
          request.open("GET", this.global_freq, false);
          request.send();
          this.freq = parseInt(request.responseText);
        },
        check_status: function(){
          if(this.freq === 0){
            this.isRunning=false;
          }
          else{
            this.isRunning=true;
          }
        }
  },
  computed: {
    label_button:{
      get(){
        return this.isRunning ? "Pause": "Reprendre";
      }
    }
  },
  mounted(){
    setInterval(this.get_global_freq, 1000);
    setInterval(this.check_status, 1000);

    
  },
  components:{
    VueNumberInput,
    Button,
    ModalPlanning,

  },
}

</script>


<template>
<div id="Gestion-container">
  <div class="title">Contrôles :</div>
  <div id="buttons">
    <Button :color="'#8dba8a'" :func="this.download" :text="'Télécharger'"></Button>
    <Button :color="'#8d90eb'" :func="button_action" :text="this.label_button"></Button>
    <Button :color="'#cf6d71'" :func="reset_func" :text="'Arrêter'"></Button>
  </div>
  <hr/>
  <ModalPlanning></ModalPlanning>
  <div id="planning">
    <div class="title">Plannification :</div>
    <div id="edit_button" v-on:click="edit">
      <img id="edit_ico" src="@/assets/edit.png"/>
    </div>
    <div id="container-freq">
      <div id="freq-input" >
        <vue-number-input id="vue-number-input" v-model="freq" :placeholder="'Fréquence'" :min="0" :max="Infinity" :rounded="true" inline controls readonly @update:model-value="onUpdate"></vue-number-input>
      </div>
    </div>
  </div>
</div>

</template>

<style lang="scss" scoped>
    #edit_ico{
      color: white;
      height: 30px;
      width: auto;
      object-fit: cover;
    }
    #edit_button{
      position: absolute;
      line-height: 0px;
      top:0px;
      right:45px;
      width: 30px;
      height: 30px;
      border-radius: 10px;
      padding: 5px;
    }
    #edit_button:hover{
      background-color: rgba($color: #000000, $alpha: 0.1);
      cursor: pointer;
    }
    #planning{
      position: relative;
      height: 100%;
    }
    #container-freq{
      height: 400px;
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
      margin-top: 30px;
      margin-left: 15px;
      margin-right: 15px;
      margin-bottom: 30px;
      height: 1px;
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