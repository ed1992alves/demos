<template>
  <div id="app">
    
    <div class="container" @click="highlightingElements">
      <img type="image/svg+xml" v-bind:src="image" >
      <vertical-navigation ref="nav" @changeOption = "changeBackground"/>
    </div>
    
  </div>
</template>

<script>
import VerticalNavigation from './components/VerticalNavigation';
import image1 from "./assets/images/background1.svg";
import image2 from "./assets/images/background2.svg";
import image3 from "./assets/images/background3.svg";
import image4 from "./assets/images/background4.svg";
import image5 from "./assets/images/background5.svg";

export default {
  data: function(){
    return {
      image: image1,
      option: 0
    }
  },
  name: 'App',
  components: {
    VerticalNavigation
  }, 
  methods:{
    changeBackground: function(option){
      this.option = option;
    },
    highlightingElements: function(){
       this.$refs.nav.highlighting = true; 
      setTimeout(()=>{ 
        this.$refs.nav.highlighting = false; 
        }, 500);
       
    }
  },
   watch: {
    option: function(){
      this.image = image5;
      if(this.option == 0)
      this.image = image1;
      if(this.option == 1)
      this.image = image2;
      if(this.option == 2)
      this.image = image3; 
      if(this.option == 3)
      this.image = image4; 
    }
   }
}
</script>

<style lang="scss">
@import "./assets/styles/main.scss";
 </style>
