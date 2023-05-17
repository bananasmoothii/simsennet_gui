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
        const dict = {"add":[]};
        const url = "http://ssngwtd.loria.fr/sendTasks.php"
        const length = this.frequency_planning.length;

        let date = new Date(this.planning_date_string[0][0]);
        dict["add"].push({"initTime": date.toISOString(), "freq":this.frequency_planning[0]});


        for(let i = 1; i <length; i++){
            if(this.planning_date_string[i-1][1] != "" &&  this.frequency_planning[i] != ""){
                let date = new Date(this.planning_date_string[i-1][1])
                dict["add"].push({"initTime": date.toISOString(), "freq":this.frequency_planning[i]});
            }
        }
        date = new Date(this.planning_date_string[length-1][1]);
        dict["add"].push({"initTime": date.toISOString(), "freq":0});

        let request = new XMLHttpRequest();
        request.open("POST", url, false);
        request.send("?json="+encodeURIComponent(JSON.stringify(dict)));
        console.log(request.responseText)
        
    },
    changed: function(){
        this.isChanging = false;
    },
    updateTime: function(){
        if(this.isChanging != true){
            const today = new Date();
            const time_chosen = new Date(this.planning_date_string[0][1])
            if (today.getTime() > time_chosen.getTime()){
                this.planning_date = today;
                this.planning_date_string[0][1] = this.planning_date.toString();
            }
            else{
                this.planning_date_string[0][1] = time_chosen.toString();
            }
        }   
    },
    opened: function(){
        this.isChanging = true; 
    },
    add: function(){
        const last_index = this.planning_date_string.length
        this.planning_date_string.push(["", ""]);
        this.frequency_planning.push(0)
    },
    delete_item: function(i){   
        this.planning_date_string.splice(i,1);
    }
  },
  data(){
    return {
        planning_date: new Date(),
        planning_date_string: reactive([]),
        frequency_planning: reactive([]),
        isChanging: false,
    
    }
  },
  beforeMount() {
    this.planning_date_string.push([this.planning_date.toString(), ""]);
  },
  mounted() {
        setInterval(this.updateTime, 500);
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
                    <span class="remove"></span>
                </div>
                <div class="line" v-for="i in planning_date_string.length" :key="i">
                    <span class="start" v-if="i-1 === 0"><VueDatePicker class="timepicker" v-model="planning_date_string[0][0]" :clearable="false" :min-date="new Date()" prevent-min-max-navigation  disable-year-select @update:model-value="changed" @open="opened"></VueDatePicker></span>
                    <span class="start" v-else><VueDatePicker class="timepicker" v-model="planning_date_string[i-2][1]" :min-date="planning_date_string[i-2][1]" :clearable="false" prevent-min-max-navigation  disable-year-select></VueDatePicker></span>
                    <span class="freq"><vue-number-input v-model="frequency_planning[i-1]" class="input" :placeholder="'Fréquence'" :min="0" :max="Infinity" :rounded="true" inline controls></vue-number-input></span>
                    <span class="end" v-if="i-1 === 0"><VueDatePicker class="timepicker" v-model="planning_date_string[i-1][1]" :min-date="planning_date_string[0][0]" :clearable="false" prevent-min-max-navigation  disable-year-select></VueDatePicker></span>
                    <span class="end" v-else><VueDatePicker class="timepicker" v-model="planning_date_string[i-1][1]" :min-date="planning_date_string[i-2][1]" :clearable="false" prevent-min-max-navigation  disable-year-select></VueDatePicker></span>
                    <span class="remove" v-if="i-1 != 0"><img class="remove_img" src="@/assets/delete.png" v-on:click="delete_item(i-1)"></span>
                    <span class="remove" v-else></span>
                </div>

                <div class="line">
                    <span class="start"></span>
                    <span class="freq"><Button id="add" :color="'#8dba8a'" :func="add" :text="'Ajouter'"></Button></span>
                    <span class="end"></span>
                    <span class="remove"></span>
                </div>  
            </div>  
            <div id="footer">
                <Button id="cancel" :color="'#cf6d71'" :func="quit" :text="'Annuler'"></Button>
                <Button id="validate" :color="'#8dba8a'" :func="submit" :text="'Valider'"></Button> 
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
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
    width: 30%;
}
.input{
    margin: 5px;   
}
.freq{
    align-self: baseline;
    margin: auto;
    height: 100%;
    width: 30%;
}
.end{
    margin: auto;
    height: 100%;
    width: 30%;
}
.remove{
    width: 10%;
    height: 100%;
}
.remove_img{
    height: 25px;
    width: auto;
    object-fit: cover;
    border-radius: 10px;
    padding: 5px;
    margin-top: 8px;
}
.remove_img:hover{
    background-color: rgba($color: #000000, $alpha: 0.1);
    cursor: pointer;
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