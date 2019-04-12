<template>
    <div class="sec_nav_container" :class="{show: show}">
        <div v-for="(item,key) in items" :class="{selected: key ==index}" class="option" @click.stop="clickItem(key)">{{item}}</div>       
        <div class="selected_secondary_border"></div>
    </div>
</template>
<script>

export default {
 data: function () {
            return {
                 items:[],
                 show:false,
                 index: 0
            }
    },
    methods:{
        clickItem: function(index){

            this.$emit('clickedSecondary', index);
            this.selectedOption(index);
            
        },
        selectedOption(index){
            
            let left = $(".sec_nav_container div:nth-child("+(index+1)+")").position().left;
            let width = $(".sec_nav_container div:nth-child("+(index+1)+")").outerWidth();

            $(".sec_nav_container .selected_secondary_border").css("left", left);
            $(".sec_nav_container .selected_secondary_border").css("width", width);
        }
    }

 }

</script>