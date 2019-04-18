<template>
  <div class="datepicker">
      <div class="label">Date </div>
      
      <div class="datepicker-container" :class="{ open: show, 'closing': closing }" @click="showDatePicker">
        <input  disabled="disabled" v-model="submitedDate">
        <div class="calendar_img"></div>
      </div>
      <div :class="{open:show}" type="text" id="datepicker">
     

        <div class="selected_date" :class="{ error: error }">
            <div class="dayOfWeek">{{dayOfWeek}}</div>
            <input v-model="currentDate"  @input="validateDate">
        </div>

        <div class="secondary_controls">
            <div class="change_month">
                <button class="previous_month" @click="reduceMonth(1)"></button>
                <div class="month">{{month}}</div>
                <button class="next_month" @click="advanceMonth(1)"></button>
            </div>
           <div class="change_year">
            <button class="previous_year" @click="reduceMonth(12)"></button>
            <div class="year">{{year}}</div>
            <button class="next_year" @click="advanceMonth(12)"></button>
            </div>
        </div>
        <div class="controls">
            <button class="cancel" @click="cancel">Cancel</button>
            <button class="submit" @click="submit">Apply</button>
        </div>
          
      </div>

  </div>
</template>

<script>


import 'jquery-ui/ui/widgets/datepicker';
import 'jquery-ui/themes/base/all.css';
import moment from 'moment'

var dayOfWeek= ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
var months = ['Jan', 'Feb' , 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

export default {
    data: function () {
        return{
            show:false,
            currentDate: "",
            dayOfWeek: "",
            submitedDate:"",
            month: "", 
            year:"",
            error:false, 
            size:0,
            closing: false
        };
    },
    mounted: function(){

        $( "#datepicker" ).datepicker({
            showOtherMonths: true,
            dayNamesMin:[ "S", "M", "T", "W", "T", "F", "S" ],
            onSelect: (value, date) => { this.selectDate(value); } 
        });

        this.submitedDate = this.convertDateToShow($( "#datepicker" ).datepicker( "getDate" ));
        this.month = $("#datepicker .ui-datepicker-month").html();
        this.year = $("#datepicker .ui-datepicker-year").html();
        

    },

    methods: {
        selectDate: function(value){
            this.dayOfWeek = dayOfWeek[$( "#datepicker" ).datepicker( "getDate" ).getDay()];
            this.currentDate = this.convertDateToShow($( "#datepicker" ).datepicker( "getDate" ));
            this.error = false;
        },

        submit: function(){
            this.submitedDate = this.currentDate;
            this.closeDatePicker();
            
        },

        cancel: function(){
            this.currentDate = this.submitedDate;
            $( "#datepicker" ).datepicker( "setDate", this.convertDateToDatePicker(this.currentDate));
            this.closeDatePicker();
            
        },

        closeDatePicker: function(){
            this.show=false;
            $("#datepicker").height("0px");
            this.closing=true;
            setTimeout( () => { 
                this.closing =false;
            }, 500);
        },

        showDatePicker: function(){
            if(!this.show){
                this.dayOfWeek = dayOfWeek[$( "#datepicker" ).datepicker( "getDate" ).getDay()];
                this.currentDate = this.convertDateToShow($( "#datepicker" ).datepicker( "getDate" ));
                this.show=true;
                $("#datepicker").height($("#datepicker .ui-datepicker").height()+210+"px");
            }
        },

        validateDate: function(){

            let date = this.convertDateToDatePicker($(".selected_date input").val());
            this.error = false;

            if(!moment(date).isValid()){
                this.error = true;
                date = this.convertDateToDatePicker(this.submitedDate);
            }


            $( "#datepicker" ).datepicker( "setDate",  date);
            this.month = $("#datepicker .ui-datepicker-month").html();
            this.year = $("#datepicker .ui-datepicker-year").html();
            this.dayOfWeek = dayOfWeek[$( "#datepicker" ).datepicker( "getDate" ).getDay()];
        },

        convertDateToDatePicker: function(date) {

            let day, month, year;
            var res = date.split(" ");

            day = res[0];
            day = (day <10 && day.length < 2) ? "0" + day : day;

            if(res[1])
            month = this.convertMonth(res[1].replace(',', ''));

            if(res[2])
            year = res[2];

            if(year.length != 4)
            return false;

            return month+"/"+ day+"/"+year;
                
        },

        convertMonth: function(month){
            let aux;

            if (month >0 && month <= 12){
                month = (month <10 && month.length < 2) ? "0" + month : month;
                return month;
            }

            $.map( months, function( val, i ) {
                if(val == month)
                {
                    aux= i+1;
                }
                    
            });

            aux = (aux <10) ? "0" + aux : aux;
            return aux;
        },

        convertDateToShow: function(dateObject) {
            var d = new Date(dateObject);
            var day = d.getDate();
            var month = months[d.getMonth()];
            var year = d.getFullYear();
            if (day < 10) {
                day = "0" + day;
            }
            if (month < 10) {
                month = "0" + month;
            }
            var date = day + " " + month + ", " + year;

            return date;
        },

        reduceMonth: function(aux){
            for(let i=0; i< aux; i++ )
            $("#datepicker .ui-datepicker-prev span").trigger( "click" );

            this.month = $("#datepicker .ui-datepicker-month").html();
            this.year = $("#datepicker .ui-datepicker-year").html();
            $("#datepicker").height($("#datepicker .ui-datepicker").height()+210+"px");
        },


        advanceMonth: function(aux){
            for(let i=0; i< aux; i++ )
            $("#datepicker .ui-datepicker-next span").trigger( "click" );

            this.month = $("#datepicker .ui-datepicker-month").html();
            this.year = $("#datepicker .ui-datepicker-year").html();
            $("#datepicker").height($("#datepicker .ui-datepicker").height()+210+"px");
        }
    }

};

</script>