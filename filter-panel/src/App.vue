<template>
  <div id="app" v-on:click="closeDropdowns" >
    <div class="container" >
      <object :data="image1"></object>
      <filters ref="painel"></filters>
      <object :data="image2"></object>
    </div>

  </div>
</template>

<script>

import filters from './Filters.vue';
import image1 from "./assets/images/background1.svg";
import image2 from "./assets/images/background2.svg";

export default {
  data:  function(){
    return {
      image1: image1,
      image2: image2,
    }
  },
  name: 'app',
  components: {filters},
  methods:{
    closeDropdowns: function (){
      this.$refs.painel.highlighting = true;
      setTimeout(()=>{ 
        this.$refs.painel.highlighting = false; 
        }, 500);
      this.$refs.painel.closeDropdowns();
  }
  }
}

</script>

<style lang="scss">
@import "./assets/styles/main.scss";
 </style>
