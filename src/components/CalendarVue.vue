<template>
   <div>
    <div class="calendar">
      <div class="header">
        <button @click="prevMonth">&lt;</button>
        <h2>{{ month }} {{ year }}</h2>
        <button @click="nextMonth">&gt;</button>
      </div>
      <table>
        <thead>
          <tr>
            <th v-for=" (day , index)  in daysOfWeek" :key="index" >{{ day }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(week, index) in weeks" :key="index">
            <td v-for=" (day , index ) in week" :key="index" :class="{blue: isActiveDay.number == day.number && day.number && isActiveDay.date == day.date, redBorder: thisDay == day.number && day.date.getFullYear() == 2023 && day.date.getMonth() == new Date().getMonth() , pinkcolor:index%7 == 6  }" @click="selectDate(day)">
              {{ day.number }}
              <div v-for="(icon , index) in imgmass" :key="index" class="dataImg" >
                <img v-if="icon.kun == day.number && icon.oy == day.date.getMonth() && icon.yil == day.date.getFullYear()" :src="icon.imgname" />
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div v-if="iconbocshow" class="container2">
        <div class="iconbox">
            <img v-for="(image , index) in icons" :key="index"  :src="image.imgname" @click="addIcon(image)" />
        </div>
       <button @click="Saqla()" class="btnstyle">Saqla</button>
    </div>
   </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        daysOfWeek: ['РН', 'ВТ', 'СР', 'ЧТ', 'ПТ', 'СБ', 'ВС'],
        weeks: [],
        month: '',
        year: '',
        selectedDate: null,
        isActiveDay:0,
        thisDay:null,
        icons:[
          {imgname:require("@/assets/rasm1.png")},
          {imgname:require("@/assets/rasm2.png")},
          {imgname:require("@/assets/rasm3.png")},
          {imgname:require("@/assets/rasm4.png")},
          {imgname:require("@/assets/rasm5.png")},
          {imgname:require("@/assets/rasm6.png")},
          {imgname:require("@/assets/rasm8.png")},
        ],
        iconbocshow:false,
        imgmass:[],
        addimgIcon:"",
        addDayIcon:null
      }
    },
    mounted() {
      this.updateCalendar()
    },
    methods: {
      updateCalendar() {
        const date = new Date()
        this.month = this.getMonthName(date.getMonth())
        this.year = date.getFullYear()
        const weeks = this.getWeeks(date.getFullYear(), date.getMonth())
        this.weeks = weeks
        this.thisDay = date.getDate() 
      },
      getMonthName(month) {
        const names = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
        return names[month]
      },
      getWeeks(year, month) {
        const weeks = []
        const date = new Date(year, month, 1)
        const lastDay = new Date(year, month + 1, 0).getDate()
        let dayOfWeek = date.getDay()
        let week = []
        for (let i = 0; i < dayOfWeek; i++) {
          week.push({ number: '', active: false })
        }
        for (let i = 1; i <= lastDay; i++) {
          week.push({ number: i, active: true, date: new Date(year, month, i) })
          dayOfWeek++
          if (dayOfWeek === 7) {
            weeks.push(week)
            week = []
            dayOfWeek = 0
          }
        }
        if (week.length > 0) {
          for (let i = dayOfWeek; i < 7; i++) {
            week.push({ number: '', active: false })
          }
          weeks.push(week)
        }
        return weeks
      },
      prevMonth() {
        const date = new Date(this.year, this.getMonthNumber(this.month) - 1, 1)
        date.setMonth(date.getMonth() - 0)
        this.month = this.getMonthName(date.getMonth())
        this.year = date.getFullYear()
        this.weeks = this.getWeeks(date.getFullYear(), date.getMonth())
      },
      nextMonth() {
        const date = new Date(this.year, this.getMonthNumber(this.month) - 1, 1)
        date.setMonth(date.getMonth() + 2)
        this.month = this.getMonthName(date.getMonth())
        this.year = date.getFullYear()
        this.weeks = this.getWeeks(date.getFullYear(), date.getMonth())
      },
      selectDate(propsDay) {
      this.isActiveDay = propsDay
      this.iconbocshow = true
      this.addDayIcon = propsDay.date
      console.log("new" , propsDay.date.getMonth());
    },
    isSameDay(date1, date2) {
      if (!date1 || !date2) return false
      return date1.getFullYear() === date2.getFullYear() && date1.getMonth() === date2.getMonth() && date1.getDate() === date2.getDate()
    },
    getMonthNumber(monthName) {
      const names = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
      return names.indexOf(monthName)
    },
    Saqla(){
      this.imgmass.push({
        imgname:this.addimgIcon,
        kun:this.addDayIcon.getDate(),
        oy:this.addDayIcon.getMonth(),
        yil:this.addDayIcon.getFullYear(),

      })
      this.iconbocshow = false
    },
    addIcon(i){
        this.addimgIcon = i.imgname

    }
  },
}
</script>

<style>
.calendar {
  font-family: sans-serif;
  max-width: 500px;
  margin: 10px auto;

}
img{
  width: 40px;
  height: 40px;
  object-fit: cover;
  margin: 0 5px;
}
.dataImg{
  position: absolute;
  right: 0;
  top: 0;
}

.dataImg img{
  width: 20px;
  height: 20px;
  object-fit: cover;
}

.iconbox{
  display: flex;
  flex-direction: row;
  justify-content:space-around;
  width: 100%;
  margin-right: 10px;
}
.iconbox img:hover{
  transition: all 0.5s ease; 
  transform: scale(1.2);
}
.container2{
  height: 60px;
  width: 500px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.btnstyle{
  background-color: teal;
  height: auto;
  font-size: 20px;
  cursor: pointer;
  padding: 0 20px;
  height: 40px;
  border-radius: 8px;
}
.header {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

button {
  border: none;
  color: #111;
  background-color: transparent;
  font-size: 1.5rem;
  cursor: pointer;
}

h2 {
  font-size: 1.5rem;
}

table {
  width: 100%;
  /* border-collapse: collapse; */
}

th {
  text-align: center;
  padding: 5px;
  height: 20px;
}

td {
  border: 1px solid #ccc;
  text-align: center;
  padding: 5px;
  cursor: pointer;
  height: 50px;
 border: 5px solid #fff;
 border-radius: 10px;
 background-color: rgba(230, 230, 230, 0.2);
 position: relative;
 top: 0;
 left: 0;
}

td.disabled {
  color: #ccc;
  cursor: default;
}

td.selected {
  background-color: #ccc;
}

.pinkcolor{
  background-color: darksalmon;
}
.blue{
  border: 2px solid teal !important;

}
.redBorder{
  background-color: teal;

}

</style>

  