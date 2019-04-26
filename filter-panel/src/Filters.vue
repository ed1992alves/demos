<template>
  <div id="filters_container" >
      <div class="filter_results">
        <div :class="['filter_open', {press: press},[ highlighting ? 'highlighting' : '']]" v-on:click.stop="changeVisibility" @mousedown="press=true" @mouseup="press=false">Filters</div>
        <div class="filter_result"> <span class="number_results"><span class="bold">{{results}}</span></span> of 2000</div>
      </div>
      <div :class="['filters', {open:'open'}]">
        <dropdown_simple ref="dropdown1" v-on:clicked="handleClickInParent" v-on:changed="valueChanged()" title="Status" order="dropdown1" v-bind:comTime="true" v-bind:firstOption="items1[0].value"  v-bind:items="items1"> </dropdown_simple>
        <dropdown_complex ref="dropdown2" v-on:clicked="handleClickInParent" v-on:changed="valueChanged()" title="Country"  order="dropdown2" v-bind:comTime="true"  v-bind:items="items2"> </dropdown_complex>
        <dropdown_simple ref="dropdown3" v-on:clicked="handleClickInParent" v-on:changed="valueChanged()" title="Priority"  order="dropdown3" v-bind:comTime="true" v-bind:firstOption="priority[0].value"  v-bind:items="priority"> </dropdown_simple>
        <dropdown_simple ref="dropdown4" v-on:clicked="handleClickInParent" v-on:changed="valueChanged()" title="Event type"  order="dropdown4" v-bind:comTime="true" v-bind:firstOption="items4[0].value"  v-bind:items="items4"> </dropdown_simple>
        <div class="reset" v-bind:class="{ 'disabled': !valuesChanged}" v-on:click.stop="valuesChanged ? resetFilters() : ''">Reset</div>
      </div>
      <div class="view_options">
          <div> <dropdown_simple ref="dropdown5" v-on:clicked="handleClickInParent" title="Sort By" order="dropdown5" v-bind:comTime="true" v-bind:firstOption="sortBy[0].value"  v-bind:items="sortBy"> </dropdown_simple></div>
            <div class="multibutton_view" >  <img src="./assets/images/list.svg"><img src="./assets/images/cards.svg"> </div>
      </div>
      <div class="sortBy"></div>

  </div>
</template>

<script>

import dropdown_simple from './DropdownSimple.vue';
import dropdown_complex from './DropdownComplex.vue';
import json from './assets/json/data.json'

export default {
 data: function () {
            return {
                results: 10,
                valuesChanged: false,
                open:false,
                items1: json.items1,
                items2: json.items2,
                priority: json.priority,
                items4: json.items4,
                sortBy: json.sortBy,
                highlighting: false,
                press:false
            }
         },
  components: {dropdown_simple, dropdown_complex},
  mounted() {
            console.log(this.open);
        console.log(this.highlighting);
    },
  methods: {
      closeDropdowns: function(){
          this.$refs.dropdown1.closeDropdown();
          this.$refs.dropdown2.cancel();
          this.$refs.dropdown3.closeDropdown();
          this.$refs.dropdown4.closeDropdown();
          this.$refs.dropdown5.closeDropdown();
    },

    valueChanged: function(arg){
        let aux1 = this.$refs.dropdown1.selected != this.$refs.dropdown1.items[0].value;
        let aux2 = this.$refs.dropdown2.checkReset();
        let aux3 = this.$refs.dropdown3.selected != this.$refs.dropdown3.items[0].value;
        let aux4 = this.$refs.dropdown4.selected != this.$refs.dropdown4.items[0].value;
        
        this.valuesChanged = false; 

        if(aux1 || aux2 || aux3 || aux4)
        this.valuesChanged = true;
        
        $('.number_results').addClass('highlighted');

        setTimeout(()=> $('.number_results').removeClass('highlighted'), 1000)

        this.results = Math.ceil(Math.random() * 2000);
    },
    
    handleClickInParent: function(dropdown){
        if(dropdown != "dropdown1")
        this.$refs.dropdown1.closeDropdown();
        if(dropdown != "dropdown2")
        this.$refs.dropdown2.cancel();
        if(dropdown != "dropdown3")
        this.$refs.dropdown3.closeDropdown();
        if(dropdown != "dropdown4")
        this.$refs.dropdown4.closeDropdown();
        if(dropdown != "dropdown5")
        this.$refs.dropdown5.closeDropdown();
    }, 

    resetFilters: function(){
        this.$refs.dropdown1.resetOption();
        this.$refs.dropdown2.resetOption();
        this.$refs.dropdown3.resetOption();
        this.$refs.dropdown4.resetOption();

        this.closeDropdowns();
    },

    changeVisibility: function(){    
        
        if(this.open){
            $(".filters").addClass("open");
            this.open =false;

        }

        else{
             $(".filters").removeClass("open");
             this.open=true;
        }

        this.closeDropdowns();

    }

  }
  
}

</script>

<style lang="scss">
 </style>