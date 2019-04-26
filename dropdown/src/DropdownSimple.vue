<template>
    <div> 
        <div class="dropdown_title">{{title}}</div>
        <button :class="['dropdown', order,  { 'selected': visibility}, {'highlighting': highlighting}]" type="button" name="button" v-on:click.stop="visibilityChange()">
                       
             <div class="selected"> {{selected}}</div>
             <div class="chevron" v-bind:class="{ 'up': upChevron}"></div>
        </button>
        <div id="dropdown-simple" v-bind:class="['dropdown-list', dropdown, {'partial': show =='partial'}]">
            <ul><li v-bind:class="[item.value , {'selected': item.selected}]" v-for= "item in items" v-on:click.stop v-on:mouseup.stop=" !item.selected ? changeOption(item) : ''">{{item.value}}</li> </ul>
        </div>
    </div>
</template>

<script>
export default {
 data: function () {
            return {
                visibility: false,
                upChevron: false, 
                id_timeout: 0,
                dropdown: 'down',
                show:'all',
                dropdownHeight: 0,
                selected: "",
                highlighting:false,
            }
        },
        props: ['order', 'items', 'firstOption', 'comTime', 'title'],
        mounted() {
                this.selected = this.firstOption;
          },
        methods:{
            handleResize: function(){
                let spaceTop = $("."+this.order + "+ .dropdown-list").position().top - window.scrollY;
                let spaceDown = $(window).height() - spaceTop;
                this.dropdownHeight = (this.items.length) * 32 + 40;

                if (spaceDown - this.dropdownHeight >0 ){
                    $("."+this.order + "+ .dropdown-list").css("height", this.dropdownHeight);
                    this.dropdown = "down";
                    this.show= "all";
                    return;
                }

                if( spaceTop - this.dropdownHeight >0){
                    var top = this.dropdownHeight;
                    $("."+this.order + "+ .dropdown-list").css( "margin-top", -top);
                    $("."+this.order + "+ .dropdown-list").css("height", this.dropdownHeight);
                    this.dropdown="up";
                    this.show="all";
                    return;
                }

                if( spaceDown > spaceTop){
                    $("."+this.order+"+ .dropdown-list" ).css("height", spaceDown-30);
                    $("."+this.order + "+ .dropdown-list ul").scrollTop(0);
                    this.dropdown="down";
                    this.show="partial";
                    return;
                }

                else{
                    $("."+this.order+"+ .dropdown-list" ).css("height", spaceTop-30 );
                    $("."+this.order + "+ .dropdown-list").css( "margin-top", -(spaceTop-30));
                    $("."+this.order + "+ .dropdown-list ul").scrollTop(0);
                    this.dropdown="up";
                    this.show="partial";
                    return;
                }
                
            },

            closeDropdown: function(){
                $("."+this.order + "+ .dropdown-list").css("height", 0);
                $("."+this.order + "+ .dropdown-list").css( "margin-top", "0px");
                this.upChevron = false;
                this.id_timeout = setTimeout(()=>{
                        this.visibility = false;
                }, 200);
            },

            visibilityChange: function(){
                
            
                clearTimeout(this.id_timeout);

                if(!this.visibility)
                {
                    this.$emit('clicked', this.order);  

                    this.handleResize();

                    this.visibility = true;
                    this.upChevron = true;

                }

                else{
                    this.closeDropdown();
                }  
            },

            resetOption: function(){
                if(this.items[0].value != this.selected){
                    this.changeOption(this.items[0]);
                }
            },

            changeOption: function (s){

                this.selected= s.value;

                this.$emit('changed'); 

                $.each( this.items, function(key, value){
                    value.selected=false;
                });
                
                s.selected= true;

                if(this.comTime){
                    setTimeout(()=>{
                        this.closeDropdown();
                        console.log("close");
                    }, 200);  
                }

                else{
                    this.closeDropdown(); 
                }            

            },
        },
}
</script>

