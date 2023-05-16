<script lang="js">
import VueNumberInput from "@/components/vue-number-input"
export default {
    name: 'Button-component',
    props:{
        text: String,
        color: String,
        func: Function,
    }
}
</script>

<template>
    <button :style="'--clr:'+this.color" v-on:click.stop="this.func"><span>{{ this.text }}</span><i></i></button>
</template>

<style lang="scss" scoped>
    button {
        position: relative;
        background: #444;
        color: #fff;
        text-decoration: none;
        text-transform: uppercase;
        border: none;
        letter-spacing: 0.1rem;
        padding: 10px 4%;
        transition: 0.2s;
        width: 250px;
        height: 40px;
    }
    button:hover {
        letter-spacing: 0.2rem;
        padding: 11px 4.1%;
        background: var(--clr);
        color: var(--clr);
        animation: box 3s infinite;
    }
    
    button::before {
        content: "";
        position: absolute;
        inset: 2px;
        background: #272822;
    }
    
    button span {
        position: relative;
        z-index: 1;
    }
    button i {
        position: absolute;
        inset: 0;
        display: block;
    }
    
    button i::before {
        content: "";
        position: absolute;
        width: 10px;
        height: 2px;
        left: 80%;
        top: -2px;
        border: 2px solid var(--clr);
        background: #272822;
        transition: 0.2s;
    }
    
    button:hover i::before {
        width: 15px;
        left: 20%;
        animation: move 3s infinite;
    }
    
    button i::after {
        content: "";
        position: absolute;
        width: 10px;
        height: 2px;
        left: 20%;
        bottom: -2px;
        border: 2px solid var(--clr);
        background: #272822;
        transition: 0.2s;
    }
    
    button:hover i::after {
        width: 15px;
        left: 80%;
        animation: move 3s infinite;
    }
    
    @keyframes move {
        0% {
        transform: translateX(0);
        }
        50% {
        transform: translateX(5px);
        }
        100% {
        transform: translateX(0);
        }
    }
    
    @keyframes box {
        0% {
        box-shadow: #27272c;
        }
        50% {
        box-shadow: 0 0 25px var(--clr);
        }
        100% {
        box-shadow: #27272c;
        }
    }
</style>