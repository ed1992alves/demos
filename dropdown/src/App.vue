<template>
  <div id="app" v-on:click="closeDropdowns" >
    <div class="container" >
      <object :data="image1"></object>
      <div class="filters">
        <dropdown_simple ref="dropdown1" v-on:clicked="handleClickInParent" v-on:changed="valueChanged()" title="Status" order="dropdown1" v-bind:comTime="true" v-bind:firstOption="items1[0].value"  v-bind:items="items1"> </dropdown_simple>
        <dropdown_complex ref="dropdown2" v-on:clicked="handleClickInParent" v-on:changed="valueChanged()" title="Country"  order="dropdown2" v-bind:comTime="true"  v-bind:items="items2"> </dropdown_complex>
      </div>
      <object :data="image2"></object>
    </div>

  </div>
</template>

<script>

import dropdown_simple from './DropdownSimple.vue';
import dropdown_complex from './DropdownComplex.vue';
import image1 from "./assets/images/background1.svg";
import image2 from "./assets/images/background2.svg";
import json from './assets/json/data.json'

export default {
  name: 'app',
   data: function () {
            return {
                items1: json.items1,
                items2: json.items2,
                image1: image1, 
                image2: image2,
            }
         },
  components: {dropdown_simple, dropdown_complex},
  methods:{
    closeDropdowns: function (){
      
console.log(!this.$refs.dropdown1.visibility + '/'+ !this.$refs.dropdown2.visibility)
      if(!this.$refs.dropdown1.visibility && !this.$refs.dropdown2.visibility ){
        this.$refs.dropdown1.highlighting=true;
        this.$refs.dropdown2.highlighting=true;

        setTimeout(()=>{ 
          this.$refs.dropdown1.highlighting=false;
          this.$refs.dropdown2.highlighting=false;
          }, 500);
      }

      this.$refs.dropdown1.closeDropdown();
      this.$refs.dropdown2.cancel();
    },
    handleClickInParent: function(dropdown){
          if(dropdown != "dropdown1")
          this.$refs.dropdown1.closeDropdown();
          if(dropdown != "dropdown2")
          this.$refs.dropdown2.cancel();
      }, 
  }
}

</script>

<style lang="scss">
@import "./assets/styles/main.scss";
 </style>
