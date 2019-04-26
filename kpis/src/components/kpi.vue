<template>
  <div id="kpi" >
      <span class="title">{{label}}</span>
      <div class="info">
        <span class="icon" :class="initial_info_icon"></span>
        <span class="value">{{initial_info_value}}</span>
        <span class="units">{{initial_info_units}}</span>
      </div>
      <div class="relative" :class="relative">
          <span class="icon"></span>
           <span class="value">{{relative_value}}</span>
           <span class="units">vs last 24 hours</span>
      </div>
    


    

  </div>
</template>

<script>

export default {
 data: function () {
            return {
                
            }
         },
  props: ['label', 'initial_info_icon', 'initial_info_value', 'initial_info_units', 'relative', 'relative_value', 'relative_units'],

  mounted: function(){

    this.forceUpdate();
    
    
  },
  methods: {
      forceUpdate: function(){

        $('#kpi .icon').addClass('loading');  
        $('#kpi .value').addClass('loading');
        $('#kpi .units').addClass('loading');

          setTimeout(function(){ 
          $('.info .icon').removeClass('loading');  
          $('.info .value').removeClass('loading');
          $('.info .units').removeClass('loading');

          setTimeout(function(){ 
            $('.relative .icon').removeClass('loading');  
            $('.relative .value').removeClass('loading');
            $('.relative .units').removeClass('loading');
          }, 0);

        }, 200);
      }
    }
}

</script>

<style lang="scss">
 </style>