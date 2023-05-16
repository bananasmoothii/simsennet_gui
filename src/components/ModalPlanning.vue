<script lang="js">
import Button from "@/components/Buttons/Button.vue";
import VueNumberInput from "@/components/vue-number-input.vue";
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
import {ref, reactive} from "vue"

export default {
  name: 'ModalPlanning',

  inject: ['provideDisplayPlanning'],
  
  methods: {
    quit: function() {
        this.provideDisplayPlanning.value = false
    },
    stay: function() {
        this.provideDisplayPlanning.value = true
    },
    submit: function(){
        return 0
    },
    changed: function(){
        this.isChanging = false;
    },
    updateTime: function(){
        if(this.isChanging != true){
            const today = new Date();
            const time_chosen = new Date(this.planning_date_string[0][0])
            if (today.getTime() > time_chosen.getTime()){
                this.planning_date = today;
                this.planning_date_string[0][0] = this.planning_date.toString();
            }
            else{
                this.planning_date_string[0][0] = time_chosen.toString();
            }
            console.log(this.planning_date_string[0][0]);
        }   
    },
    opened: function(){
        this.isChanging = true; 
    },
    add: function(){
        const today = new Date(); 
        this.planning_date_string.push(["", ""]);
        this.items.push("");
        console.log(this.items);
    },
  },
  data(){
    return {
        planning_date: new Date(),
        planning_date_string: reactive([]),
        isChanging: false,
        items: [],
    }
  },
  mounted() {
        this.add();
        setInterval(this.updateTime, 1000);
  },
  components:{
    VueNumberInput,
    Button,
    VueDatePicker,
  },
}

</script>

<template>
    <div id="background" v-on:click.stop="quit" v-if="this.provideDisplayPlanning.value">
        <div id="modal" v-on:click.stop="stay">
            <div id="header">
                <div id="title">Plannification :</div>
                <img id="close" v-on:click.stop="quit" src="@/assets/close_icon.png"/>
            </div>
            <div id="content">
                <div class="line" style="text-decoration: underline;">
                    <span class="start">Heure de départ</span>
                    <span class="freq">Fréquence</span>
                    <span class="end">Heure de fin</span>
                </div>
                <div class="line" v-for="(item,index) in items" :key="item">
                    <span class="start" v-if="index === 0"><VueDatePicker class="timepicker" v-model="planning_date_string[0][0]" :clearable="false" :min-date="new Date()" prevent-min-max-navigation  disable-year-select enable-seconds @update:model-value="changed" @open="opened"></VueDatePicker></span>
                    <span class="start" v-else><VueDatePicker class="timepicker" v-model="planning_date_string[index][0]" :clearable="false"  prevent-min-max-navigation  disable-year-select enable-seconds ></VueDatePicker></span>
                    <span class="freq"><vue-number-input class="input" :placeholder="'Fréquence'" :min="0" :max="Infinity" :rounded="true" inline controls></vue-number-input></span>
                    <span class="end"  v-if="index === Object.keys(items).length - 1">Jamais</span>
                    <span class="end" v-else><VueDatePicker class="timepicker" v-model="planning_date_string[index][1]" :clearable="false" prevent-min-max-navigation  disable-year-select enable-seconds></VueDatePicker></span>
                </div>

                <div class="line">
                    <span class="start"></span>
                    <span class="freq"><Button id="add" :color="'#8dba8a'" :func="add" :text="'Ajouter'"></Button></span>
                    <span class="end"></span>
                </div>  
            </div>  
            <div id="footer">
                <Button id="cancel" :color="'#cf6d71'" :func="quit" :text="'Annuler'"></Button>
                <Button id="validate" :color="'#8dba8a'" :func="submit" :text="'Valider'"></Button> 
            </div>
        </div>
    </div>
</template>

<style scoped>
#add{
    width: 50%;
}
.timepicker{
    margin: auto;
    margin-top: -2px;
    width: 225px;
    height: 40px;
}
.line{
    padding-top: 5px;
    padding-bottom: 5px;
    height: 50px;
    line-height: 2.5;
    display: flex;
    flex-direction: row;
    flex-basis: auto;
    
}
.start{
    align-self: center;
    margin: auto;
    height: 100%;
    width: 33%;
}
.input{
    margin: 5px;   
}
.freq{
    align-self: baseline;
    margin: auto;
    height: 100%;
    width: 33%;
}
.end{
    margin: auto;
    height: 100%;
    width: 33%;
}
#background {
    position: absolute;
    top:0;
    left:0;
    z-index: 2;
    background-color: #616161c7;
    width: 100%;
    height: 100%;
}
#modal{
    position: relative;
    top:10%;
    left:25%;
    border-radius: 15px;
    box-shadow: 0 0 2em black;
    z-index: 3;
    background-color: white;
    width: 50%;
    height: 80%;
    
}
#header{
    z-index: 4;
    display: block;
    height: 10%;
}
#title{
    width: auto;
    padding-top: 20px;
    padding-bottom: 20px;
    font-size: 2.5em;
    border-bottom: 1px black solid;
    margin-left: 20px;
    margin-right: 20px;
    text-align: center;
    text-transform:uppercase;
}
#close{
    position: absolute;
    top:20px;
    right:20px;
    height: 50px;
    width: auto;
    object-fit: cover;
}
#close:hover{
    filter: brightness(85%);
    cursor: pointer;
}
#content{
    margin-top: 10px;
    height: 75%;
    font-size: 1.25em;
    text-align: center;
    overflow: scroll;
}
#footer{
    position: relative;
    height: 10%;
}
#cancel{
    position: absolute;
    bottom: 25px; left: 25px;
    width: 150px;
}
#validate{
    position: absolute;
    bottom: 25px; right: 25px;
    width: 150px;
}

</style>