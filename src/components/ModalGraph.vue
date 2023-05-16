<script lang="js">
export default {
  name: 'ModalGraph',

  props:{
    node_name: String,
  },
  inject: ['provideDisplayGraph'],
  
  methods: {
    quit: function() {
        this.provideDisplayGraph.value = false
    },
    stay: function() {
        this.provideDisplayGraph.value = true
    }
}
}

</script>

<template>
    <div id="background" v-on:click.stop="quit" v-if="this.provideDisplayGraph.value">
        <div id="modal" v-on:click.stop="stay">
            <div id="header">
                <div id="title">{{ node_name }}</div>
                <img id="close" v-on:click.stop="quit" src="@/assets/close_icon.png"/>
            </div>
            <div id="content">Work in progress ..</div>
                
        </div>
    </div>
</template>

<style scoped>
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
    position: absolute;
    top:15%;
    left: 15%;
    border-radius: 15px;
    box-shadow: 0 0 2em black;
    z-index: 3;
    background-color: white;
    width: 70%;
    height: 70%;
    
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
    height: 100%;
    margin-top: 50px;
    font-size: 1.25em;
    text-align: center;
}

</style>