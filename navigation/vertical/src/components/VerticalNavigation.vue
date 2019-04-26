<template>
    <div class="vertical_navigation">
        <ul>
            <li v-for="(value,key) in nav_elements" @click.stop="change(key)"  @mousedown="value.press=true" @mouseleave="value.press=false" @mouseup="value.press=false" :class="[{selected : value.selected}, {press:value.press}, {highlighting : highlighting}]">
                <object :data="value['icon']" width="32px" height="32px"></object>
                <div>{{value.title}}</div>
            </li>
        </ul>
    </div>
</template>

<script>
import icon1 from '../assets/images/icon1.svg';
import icon2 from '../assets/images/icon2.svg';
import icon3 from '../assets/images/icon3.svg';
import icon4 from '../assets/images/icon4.svg';
import icon5 from '../assets/images/icon5.svg';

export default {
  data: function(){
    return {
        nav_elements:[
            {
                title: "Vertical Item 1",
                icon: icon1,
                selected:true,
                press:false,
            },
            {
                title: "Vertical Item 2",
                icon: icon2, 
                selected:false,
                press:false,
            },
           {
                title: "Vertical Item 3",
                icon: icon3, 
                selected:false,
                press:false,
            },
            {
                title: "Vertical Item 4",
                icon: icon4,
                selected:false,
                press:false,
            },
            {
                title: "Vertical Item 5",
                icon: icon5, 
                selected:false,
                press:false,
            },
          
        ],
        highlighting : false,
    }
  },
    mounted: function(){

        setTimeout(function(){ 
        let elements = $('.vertical_navigation object')[0].contentDocument.getElementsByTagName('path');
            for (var item of elements) {
                item.style.fill = '#FFF'
            }

        }, 200);
    },
  methods:{
      change: function(key){
          $.map(this.nav_elements, function(val,i){
              val.selected = false;
          })

          this.nav_elements[key].selected = true;

            $('.vertical_navigation object').map((key,value) => {

                    let elements = value.contentDocument.getElementsByTagName('path'); 
                    for (var item of elements) {
                        item.style.fill = '#000'; 
                        item.style.transition = 'fill 0.1s linear'; 
                    }

            }); 

            let elements = $('.vertical_navigation object')[key].contentDocument.getElementsByTagName('path');
            for (var item of elements) {
                item.style.fill = '#FFF'
                item.style.transition = 'fill 0.2s linear'; 
            }

            
                
            this.$emit('changeOption', key);

            
    },

  }
}

</script>