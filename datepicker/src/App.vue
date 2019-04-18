<template>
  <div id="app" >
    <div class="container" >
      <object :data="header"></object>
        <date_picker class="date_picker" ref="tab"> </date_picker>
      <object :data="image"></object>
      <div class="button">
        <div class="info-text">Change DatePicker</div>
    </div>
    </div>
    

  </div>
</template>

<script>

import date_picker from './DatePicker.vue';
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
                image: image1
            }
         },
  components: {date_picker},
  methods:{
    changeBackground: function(item){

      this.image = image4;
      if(item == 1)
      this.image = image1;
      if(item == 2)
      this.image = image2;
      if(item == 3)
      this.image = image3; 

    }
  }
}

</script>

<style lang="scss">
@import "./assets/styles/main.scss";
 </style>
