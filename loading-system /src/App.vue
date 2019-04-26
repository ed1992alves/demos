<template>
  <div id="app" >
    <div class="container" >
      <img :src="image">
      <div class="button-container">
        <button class="info-text" @click="changeGlobal">Global Loading</button>
        <button class="info-text" @click="changeComponent">Widget Loading</button>
        <button class="info-text" @click="changeButton(true)">Button Loading</button>
       </div>
        <div class="loader-global" :class="{active: global}">
          <span class="bar"></span>
          <span class="bar"></span>
          <span class="bar"></span>
        </div>
     
        <div class="loader-component" :class="{active: component}">
          <span class="bar"></span>
          <span class="bar"></span>
          <span class="bar"></span>
        </div>

        <div @click="changeButton(false)" class="loader-button-container blue" :class="{global: global, active: button}">{{label}}
          <div class="loader-button">
            <span class="bar"></span>
            <span class="bar"></span>
            <span class="bar"></span>
          </div>
        </div>
    </div>
    

  </div>
</template>

<script>

import image from "./assets/images/background.svg";

var label = "Submit";

export default {
  name: 'app',
   data: function () {
            return {
              image: image,
              global: false,
              component:false,
              button:false,
              label: "",

            }
         },
  mounted: function(){
    this.label =label;  },
  methods:{
   changeGlobal: function(){
     this.global = !this.global;
     this.component = false;
     this.button = false;
     $("html, body").animate({ scrollTop: 0});
   },
   changeComponent: function(){
     this.component = !this.component;
     this.global = false;
     this.button = false;

    $("html, body").animate({ scrollTop: $(".loader-component").offset().top });


   },
   changeButton: function(scroll){
     this.button = !this.button;
     this.global = false;
     this.component = false;
     if(scroll)
     $("html, body").animate({ scrollTop: $(document).height()-$(window).height() });

   }
  }
}

</script>

<style lang="scss">
@import "./assets/styles/main.scss";
 </style>
