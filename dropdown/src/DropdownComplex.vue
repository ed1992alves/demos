<template>
    <div> 
        <div class="dropdown_title">{{title}}</div>
        <button :class="['dropdown', 'dropdown_search', order,  { 'selected': visibility}, {'highlighting': highlighting}]" type="button" name="button" v-on:click.stop="visibilityChange()">
            <span class="bold">{{selected}}</span><span v-if="selected !='All'"> of {{items.length}}</span>
            <div class="chevron" v-bind:class="{ 'up': upChevron}"></div>
        </button>
        <div v-on:click.stop id="dropdown-simple" v-bind:class="['dropdown-list', dropdown, {'partial': show =='partial'}]">
        <div class="input_container">
                <input v-model="term"></input> 
                <div class="search-icon" @click.stop="cleanSearch()"> </div>
            </div>

        <div class="all" v-on:mouseup.stop="clickAll()"><span class="bold">{{pre_selected}} </span><span v-if="pre_selected!='All'"> of {{items.length}}</span></div>
            
        <ul>
            <li v-bind:class="[item.value , {'selected': item.selected}, {'hidden': !item.visibility}]" v-for= "item in items"  v-on:mouseup.stop=" changeOption(item)">{{item.value}}</li>
            <li class="none_results" v-if='none'> No Results </li> 
        </ul>   
            
        <div class="actionButtons">
            <button class="cancel"  @mouseup="cancel">Cancel</button>
            <button class="apply"  @mouseup="apply" > Apply  </button>
        </div>
            
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
                term: "",
                selected:"",
                dropdownHeight:0,
                pre_selected:"",
                dropdown: 'down',
                show:'all',
                highlighting:false,
                none: false
            }
        },
        props: ['order', 'items', 'comTime', 'title'],
        mounted() {
            this.selected = 1;
            this.pre_selected = 1;
            this.numberSelected();
          },
        methods:{
            handleResize: function(aux = true){

                let spaceTop = $("."+this.order).position().top - window.scrollY;
                let spaceDown = $(window).height() - spaceTop - 32;


                this.getHeight();

                if(this.term == "" && (spaceDown >= spaceTop))
                    this.dropdown="down";

                if(this.term == "" && (spaceTop > spaceDown))
                    this.dropdown="up";

                if (this.dropdown == "down"){
                    if(spaceDown - this.dropdownHeight >=0) {
                        $("."+this.order + "+ .dropdown-list").css("height", this.dropdownHeight);
                        this.show= "all";
                        return;
                    }
                        $("."+this.order+"+ .dropdown-list" ).css("height", spaceDown-30);
                        $("."+this.order + "+ .dropdown-list ul").scrollTop(0);
                        this.show= "partial";
                        return;
                }

                if  (this.dropdown == "up"){
                    if (spaceTop - this.dropdownHeight >=0){
                        $("."+this.order + "+ .dropdown-list").css("margin-top", -this.dropdownHeight);
                        $("."+this.order + "+ .dropdown-list").css("height", this.dropdownHeight);
                        this.show= "all";
                        return;
                    }

                    $("."+this.order+"+ .dropdown-list" ).css("height", spaceTop-30 );
                    $("."+this.order + "+ .dropdown-list").css("margin-top", -(spaceTop - 30));
                    $("."+this.order + "+ .dropdown-list ul").scrollTop(0);
                    this.show= "partial";
                    return;
                }
                
            },

            getHeight: function(){
                if(this.term == ""){
                    this.dropdownHeight = this.items.length*32+72*2+42 +1 ; 
                    this.none = false; 
                }

                else{
                    var aux=0;
                    $.map(this.items, (value,key)=>{
                        if(value.value.toLowerCase().includes(this.term.toLowerCase()))
                        aux++; 
                    });
                    if(aux != 0){
                        this.dropdownHeight = (aux)*32+72*2; 
                        this.none = false;
                    }
                        
                    if(aux == 0){
                        this.dropdownHeight = 32+ 72*2; 
                        this.none = true;
                    }
                }
            },

            closeDropdown: function(){
                $("."+this.order + "+ .dropdown-list").css("height", 0);
                $("."+this.order + "+ .dropdown-list").css("margin-top", "0px");
                this.upChevron = false;
                this.id_timeout = setTimeout(()=>{
                        this.visibility = false;
                    }, 200);
            },


            visibilityChange: function(){
                this.cleanSearch();
            
                clearTimeout(this.id_timeout);

                if(!this.visibility)
                {
                    this.$emit('clicked', this.order);   

                    this.handleResize();

                    this.visibility = true;
                    this.upChevron = true;

                }

                else{
                    this.cancel();
                }  
            },

            changeOption: function (s){

                s.selected= !s.selected;
                s.altered = !s.altered;

                let aux= this.numberSelected();
                if(aux == this.items.length)
                {
                    if($("."+ this.order + "+ .dropdown-list .all").attr("class").split(' ')[1] != "selected")
                        this.selectAll();
                    else 
                        this.removeAll();
                }
                    
                else
                    this.controlAllOption(aux);

            },

            controlAllOption: function(aux){
                this.pre_selected = aux;
                $("."+this.order + "+ .dropdown-list .all").removeClass("selected");
                $("."+this.order + "+ .dropdown-list .all").removeClass("none");
                if(aux == 0)
                $("."+this.order + "+ .dropdown-list .all").addClass("none");
            },

            numberSelected: function(){
                let aux=0;

                $.map(this.items, (value,key)=>{
                    if(value.selected){
                        aux++; 
                    }
                });

                return aux;
  
            },

            clickAll: function(){
                if($("."+ this.order + "+ .dropdown-list .all").attr("class").split(' ')[1] == "none")
                    return this.selectAll();
                
                return this.removeAll();

            },

            removeAll: function(){
                    $.map(this.items, (value,item)=>{
                        if(value.selected == true){
                            value.selected = false;
                            value.altered = !value.altered;
                        }
                    });

                    this.pre_selected = "All";
                    $("."+this.order + "+ .dropdown-list .all").removeClass("selected");
                    $("."+this.order + "+ .dropdown-list .all").addClass("none");
            },
            selectAll: function(){
                $.map(this.items, (value,item)=>{
                    if(value.selected != true){
                        value.selected = true;
                        value.altered = !value.altered;
                    }
                });

                this.pre_selected = "All";
                $("."+this.order + "+ .dropdown-list .all").addClass("selected");
                $("."+this.order + "+ .dropdown-list .all").removeClass("none");
            },
            cleanSearch: function(){
                this.term = "";
            },
            cancel: function(){
                $.map(this.items, function(value,key){
                    if(value.altered){
                        value.selected = !value.selected;
                    }
                    value.altered = false; 
                });

                let aux = this.numberSelected(); 
                this.selected =  aux;

                if(aux == this.items.length){
                    this.selectAll;  
                    this.selected = "All";
                }
                    
                if(aux == 0)
                    this.removeAll;
                if(aux>0 && aux < this.items.length) 
                    this.controlAllOption(aux);

                this.closeDropdown();   
            },
            apply: function(){
                $.map(this.items, function(value,key){
                    value.altered = false;
                });

                let aux = this.numberSelected(); 
                this.selected = aux;

                if(aux == this.items.length){
                    this.selectAll;  
                    this.selected = "All";
                }
                if(aux == 0)
                    this.removeAll;
                if(aux>0 && aux < this.items.length) 
                    this.controlAllOption(aux);


                this.closeDropdown();
                this.$emit('changed'); 
            },

            checkReset: function(){
                let aux = false;
                $.map(this.items, (value,key)=>{
                    if(key !=0 && value.selected == true){
                        aux = true;
                    }     
                });

                return aux;
                
            },

            resetOption: function(){
                $.map(this.items, (value,key)=>{
                    value.selected = false; 
                    value.visibility = true;
                    value.altered = false; 
                }); 

                this.items[0].selected = true; 
                this.apply();
            }
        },
        watch:{
            term: function(){

                $("."+this.order + "+ .dropdown-list .search-icon").removeClass("close");
                $("."+this.order + "+ .dropdown-list .all").removeClass("hidden");
                    if( this.term.length >0 ){
                        $("."+this.order + "+ .dropdown-list .search-icon").addClass("close");
                        $("."+this.order + "+ .dropdown-list .all").addClass("hidden");
                    }
                
                $.map(this.items, (item, value)=>{
                    item.visibility=true;

                    if(!item.value.toLowerCase().includes(this.term.toLowerCase())){
                        item.visibility=false;
                    }

                });

                this.getHeight(); 

                this.handleResize(false);
                
            }
        }
    } 
</script>