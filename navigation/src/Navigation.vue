<template>
    <div>
        <div class="nav_container" :class="{ open:open }">
            <div id='nav1' class="navigation" :class= "{ highlighting: highlighting}">
                <div v-for="(item,key) in items"  :class="[item.class, {selected: item.selected}, {pressed: item.press}]" @click.stop="clickItem(key)" @mousedown="item.press=true" @mouseup="item.press=false">{{item.label}}</div>
            </div>
            <div class="selected_border"></div>               
        </div>
        <secondary_navigation ref="sec_nav" v-on:clickedSecondary="changeIndex"></secondary_navigation>
    </div>
</template>



<script>

import secondary_navigation from './SecondaryNavigation.vue';

    export default{
         data: function () {
            return {
                items:[
                    {
                        class:"nav0",
                        label: "Analytics",
                        selected:true,
                        press:false
                    }, 
                    {
                        class:"nav1",
                        label: "Work Orders",
                        selected:false,
                        press:false,
                        secondary_navigation_index:0,
                        items:["Home", "Nodes", "Drives", "Servers", "Volumes", "Ports", "Hardware", "Software", "Pools", "Disks", "Reports", "Incidents"]
                    }, 
                    {
                        class:"nav2",
                        label: "Assets",
                        selected:false,
                        press:false  
                    },
                    {
                        class:"nav3",
                        label: "Resources",
                        selected:false,
                        press:false  
                    }]
                ,
                highlighting:false,
                open:false
            }
        },
        props: ['startingPosition'],
        mounted: function(){

            $(".navigation div.selected").removeClass("selected");
            $(".navigation div.nav"+this.startingPosition).addClass("selected");

            setTimeout(()=> {  
                this.items[this.startingPosition].selected = true;
                this.changeBar();   }, 
            300);  

        },
        methods:{
            clickItem: function(item){
                $(".navigation div.selected").removeClass("selected");
                $(".navigation div.nav"+item).addClass("selected");
                this.items.map(( value) => {
                    value.selected=false;
                });
                this.items[item].selected = true;
                this.changeBar();

                if(this.items[item].items == undefined)
                {
                    this.$emit('clicked', item);
                    this.$refs.sec_nav.show = false;
                    this.open = false;
                }

                else{
                    this.open = true;
                    this.$refs.sec_nav.items = this.items[item].items;
                    this.$refs.sec_nav.show = true;
                    this.$refs.sec_nav.index = this.items[item].secondary_navigation_index;
                    this.$emit('clicked', item+""+this.items[item].secondary_navigation_index);
                     setTimeout(()=> {  
                        this.$refs.sec_nav.selectedOption(this.items[item].secondary_navigation_index);},
                    100);  
                    
                }
                
                
            },

            changeIndex(index){
                this.items.map(( value, i) => {
                    if(value.selected)
                    {
                        value.secondary_navigation_index = index;
                        this.clickItem(i);
                        return;
                    }
                });''
                
            },

            changeBar: function(){
                let left = $(".navigation div.selected").position().left;
                let width = $(".navigation div.selected").outerWidth();

                $(".navigation + .selected_border").css("left", left);
                $(".navigation + .selected_border").css("width", width);
            }
            
        },
        components:{
            secondary_navigation
        }
    }
</script>
<style>
</style>