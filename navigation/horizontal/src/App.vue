<template>  
  <div id="app">
    <div class="dropdownMain" >
      <div class="container" @click="clickOutside" >
          <img type="image/svg+xml" v-bind:src="header" >
          <navigation ref="nav" v-bind:startingPosition="option" v-on:clicked="changeBackground"></navigation>
          <img type="image/svg+xml" v-bind:src="image" >
      </div>   
    </div>
  </div>
</template>

<script>
import navigation from './Navigation.vue';
import image1 from "./assets/images/background1.svg";
import image2 from "./assets/images/background2.svg";
import image3 from "./assets/images/background3.svg";
import image4 from "./assets/images/background4.svg";
import image5 from "./assets/images/background5.svg";
import image6 from "./assets/images/background6.svg";
import image7 from "./assets/images/background7.svg";
import image8 from "./assets/images/background8.svg";
import header from "./assets/images/header.svg";

export default {
  name: 'app',
  data: function () {
    return {
      option: 0,
      image: image1,
      header:header
    }
  },
  methods: {
    changeBackground: function(option){
      this.option = option;
    },
    clickOutside: function (){
      this.$refs.nav.highlighting = true;
      setTimeout( () => { 
        this.$refs.nav.highlighting = false; 
        }, 500);
    }
  },
  watch: {
    option: function(){
      this.image = image7;
      if(this.option == 0)
      this.image = image1;
      if(this.option == 11)
      this.image = image2;
      if(this.option == 12)
      this.image = image3;
      if(this.option == 13)
      this.image = image4;
      if(this.option == 14)
      this.image = image5;
      if(this.option == 15)
      this.image = image2;
      if(this.option == 16)
      this.image = image3;
      if(this.option == 17)
      this.image = image4;
      if(this.option == 18)
      this.image = image5;
      if(this.option == 19)
      this.image = image2;
      if(this.option == 110)
      this.image = image3;
      if(this.option == 111)
      this.image = image4;
      if(this.option == 112)
      this.image = image5;
      if(this.option == 14)
      this.image = image5;
      if(this.option == 14)
      this.image = image5;
      if(this.option == 2)
      this.image = image6; 
    }
  },
  components:{
    navigation
  }
}
</script>

<style lang="scss">
@import "./assets/styles/main.scss";
 </style>
