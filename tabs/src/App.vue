<template>
  <div id="app" >
    <div class="container" >
      <object :data="header"></object>
        <tab ref="tab" v-on:clicked="changeBackground"> </tab>
      <object :data="image"></object>
      <div class="button">
        <div class="info-text" @click="changeSize">Change to {{next_tab}} tab</div>
    </div>
    </div>
    

  </div>
</template>

<script>

import tab from './Tab.vue';
import header from "./assets/images/header.svg";
import image1 from "./assets/images/background1.svg";
import image2 from "./assets/images/background2.svg";
import image3 from "./assets/images/background3.svg";
import image4 from "./assets/images/background4.svg";
import json from './assets/json/data.json'

export default {
  name: 'app',
   data: function () {
            return {
              header: header,
                image: image1,
                tab_size: ["Small", "Medium"],
                next_tab: "Medium"
            }
         },
  components: {tab},
  methods:{
    changeBackground: function(item){

      this.image = image4;
      if(item == 1)
      this.image = image1;
      if(item == 2)
      this.image = image2;
      if(item == 3)
      this.image = image3; 

      window.moveTo( scrollY, scrollX );

    },

    changeSize: function(){

      if(this.next_tab == "Medium"){
        this.next_tab = this.tab_size[0];
        this.$refs.tab.size = this.tab_size[1];
        this.$refs.tab.changeTabSize();
      }
      else
      {
        this.next_tab = this.tab_size[1];
        this.$refs.tab.size = this.tab_size[0];
        this.$refs.tab.changeTabSize();
      }
    }
  }
}

</script>

<style lang="scss">
@import "./assets/styles/main.scss";
 </style>
