<template>
  <div>
    <div class="calendar">
      <div class="calendar-header">
        <button @click="prevMonth">&lt;</button>
        <span> {{ curretnMonth }}</span>
        <button @click="nextMonth">&gt;</button>
      </div>
      <div class="week-days">
        <div
        v-for="(day, index) in weekDays"
        :key="index"
        class="week-day">
{{ day }}
        </div>
      </div>
      <div class="calendar-body">
        <div
        v-for="(day, index) in calendarDays"
        :key="index"
        :class="{'selected': selectedDate && selectedDate.getDate() === day}"
        @click="selectDate(day)"
          class="calendar-day"
        >
        {{ day }}
        </div>
      </div>
    </div>
    <div class="language-buttons">
      <button class="button-language" @click="changeLanguage('en')">English</button>
      <button class="button-language" @click="changeLanguage('ru')">Русский</button>
    </div>
  </div>
</template>


<script>
export default {

  props:{
    initialDate:{
      type: String,
      default: "",
    }
  },

  data(){
    const curretnDate = this.initialDate ? new Date(this.initialDate) : new Date();
    return{
      curretnDate: curretnDate,
      selectedDate: curretnDate,
      language: 'ru',
      months:{
        en: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
        ru: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь']
      },
      daysOfWeek:{
        en: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
        ru: ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']
      }
    };
  },

  computed:{
    curretnMonth(){
      return this.months[this.language][this.curretnDate.getMonth()] + " " + this.curretnDate.getFullYear();
    },
    weekDays(){
      return this.daysOfWeek[this.language];
    },

    calendarDays(){
      const firsyDaysOfMonth = this.curretnDate.getDay();
      const daysInMonth = new Date(this.curretnDate.getFullYear(), this.curretnDate.getMonth() + 1, 0).getDate();
      const daysArray = [];


      for(let i = 0; i < firsyDaysOfMonth; i++){
        daysArray.push('');
      }
      for(let i = 1; i <= daysInMonth; i++){
        daysArray.push(i);
      }

      return daysArray;
    }

  },

  methods:{
    prevMonth(){
      this.curretnDate = new Date(this.curretnDate.getFullYear(), this.curretnDate.getMonth() - 1, 1)

    },
    nextMonth(){
      this.curretnDate = new Date(this.curretnDate.getFullYear(), this.curretnDate.getMonth() + 1, 1)

    },
    selectDate(day){
      if(day !== ''){
        this.selectedDate = new Date(this.curretnDate.getFullYear(), this.curretnDate.getMonth(), day)
        this.$emit("date-selected", this.selectedDate);
      }
      
    },
    changeLanguage(lang){
      this.language = lang
    },
  }



  
};
</script>

<style scoped>

.calendar {
border: 1px solid #ccc;
width: 259px;
margin: 0 auto;
font-family: Arial, sans-serif;
}

.calendar-header {
display: flex;
justify-content: space-between;
padding: 10px;
background-color: #f0f0f0;
}

.calendar-body {
display: flex;
flex-wrap: wrap;
}

.calendar-day {
width: calc(100% / 7);
padding: 3px;
text-align: center;
cursor: pointer;
}

.calendar-day:hover {
background-color: #f0f0f0;
}

.selected {
background-color: #007bff;
color: #fff;
}

.week-days {
display: flex;
}

.week-day {
width: calc(100% / 7);
padding: 5px;
text-align: center;
}


.language-buttons {

padding: 10px;
}

.button-language {
margin-left: 10px; 
padding: 5px 10px; 
background-color: #007bff; 
color: #fff; 
border: none; 
border-radius: 5px; 
cursor: pointer;
}

.button-language:hover {
background-color: #0056b3; 
}
</style>
