<template>
    <div class="tab" :class="size"> 
        <button class="selected" @click="clickItem(1)" >First Option</button>
        <button @click="clickItem(2)">Second Option</button>
        <button @click="clickItem(3)">Third Option</button>
        <div class="selected-object"></div>
    </div>
</template>

<script>

export default {
 data: function () {
            return {
                size:"Medium",
                small_width:0,
                medium_width:0
            }
        },
    mounted: function(){
        this.medium_width = this.getTabsWidth();

        this.size= "Small"; 

        setTimeout( () => { 
            this.small_width = this.getTabsWidth();
            this.setTabsWidth(this.small_width);
            this.changeBar(); 
        }, 200);

    },
        methods:{
            setTabsWidth: function(width){
                $.map( $('.tab button'), ( n, i )=> {
                $(n).width(width);
             });
            },
            getTabsWidth: function(){
                let width=0;
                let aux;
                console.log(this.size);
                $.map( $('.tab button'), ( n, i )=> {
                    aux = $(n).width();
                    console.log(aux);
                    if(aux > width){
                        width = aux;
                    }
                });
                return width;
            },
            clickItem: function(item){
                $(".tab button.selected").removeClass("selected");
                $(".tab button:nth-child("+item+")").addClass("selected");
                this.$emit('clicked', item);
                this.changeBar(item);
            },

            changeBar: function(item){
                let left = $(".tab button.selected").position().left;
                let width = $(".tab button.selected").outerWidth();
                

                $(".tab .selected-object").css("left", left);
                $(".tab .selected-object").css("width", width);

                this.$emit('clicked', item);

            },

            changeTabSize: function(){
                if(this.size =="Medium"){
                    this.setTabsWidth(this.medium_width);
                }
                else
                    this.setTabsWidth(this.small_width);

                this.changeBar();
            }
            
        }
}
</script>

