<template>
  <div id="app" >
    <div class="container">
      <object :data="image1"></object>
      <div class="tryme" @mouseup="forceUpdate">Click here</div>
      <kpi ref="kpi1" label="Avg. Service Time" initial_info_icon="positive" initial_info_value="8.85" initial_info_units="MS" relative ="positive" relative_value="10%"></kpi>
      <kpi ref="kpi2" label="Total IOPS" initial_info_icon ="negative" initial_info_value="113,277" initial_info_units="" relative ="negative" relative_value="0.15%"></kpi>
      <object :data="image2"></object>
    </div>

  </div>
</template>

<script>

import kpi from './components/kpi.vue';
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
  components: {kpi},
  methods:{

    forceUpdate: function(){
      console.log("Entrei");
      this.$refs.kpi1.forceUpdate();
    }

  }
}

</script>

<style lang="scss">
@import "./assets/styles/main.scss";
 </style>
