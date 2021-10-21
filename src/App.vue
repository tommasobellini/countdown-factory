<template>
  <CountDownWidget :timer="counter.counterString" :state="counterState"></CountDownWidget>
</template>
<script>
import CountDownWidget from "./components/CountDownWidget.vue";
// import axios from "axios";
import moment from "moment";
moment.locale("it");
// const BASE_URL = " http://192.168.1.108:8000";
// const COUNTDOWN_API = BASE_URL + "/get_countdown/";

export default {
  name: "App",
  components: {
    CountDownWidget,
  },
  data() {
    return {
      counter: {
        name: "",
        startDate: "",
        counterString: "",
        days: 0,
        hours: 0,
        minutes: 0,
        seconds: 0,
      },
      tempCountDownTime: moment.duration(),
      counterState: false,
    };
  },
  methods: {
    startCounter() {
      var tempEndDate = Date.parse("2021-10-21 23:03:00");
      var x = moment(tempEndDate);
      var y = moment(Date.now());
      var duration = moment.duration(x.diff(y));
      console.log("temp: " + duration.days());
      this.tempCountDownTime = duration;
      setInterval(() => {
        this.tempCountDownTime = this.tempCountDownTime.subtract(1, "s");
        this.getCounter();
        this.getImage();
      }, 1000);
    },
    // getCounter(){
    //   axios.get(COUNTDOWN_API).then((resp) => {
    //     var data = resp.data;
    //     console.log(data.start_date);
    //     var tempEndDate = Date.parse(data.end_date);
    //     // var tempStartDate = Date.parse(date.start_date);
    //     var temp = moment(tempEndDate, 'YYYY-MM-DD HH:mm:ss').diff(Date.now(), 'YYYY-MM-DD HH:mm:ss');
    //     this.tempCountDownTime = temp;
    //   });
    // }
    addZero(num = "") {
      if (num.length < 2) {
        return "0" + num;
      } else return num;
    },

    getCounter() {
      var counterString =
        this.addZero(this.tempCountDownTime.days().toString()) +
        ":" +
        this.addZero(this.tempCountDownTime.hours().toString()) +
        ":" +
        this.addZero(this.tempCountDownTime.minutes().toString()) +
        ":" +
        this.addZero(this.tempCountDownTime.seconds().toString());
      this.counter.counterString = counterString;
      this.counter.days = this.tempCountDownTime.days();
      this.counter.hours = this.tempCountDownTime.hours();
      this.counter.minutes = this.tempCountDownTime.minutes();
      this.counter.seconds = this.tempCountDownTime.seconds();
      console.log(this.counter.counterString);
    },

    getImage(){
      if(this.counter.days == 0 && this.counter.hours==0 && this.counter.minutes==0 && this.counter.seconds==0){
        this.counterState = true;
        console.log(this.counterState);
      }
      else {
        this.counterState = false;
      console.log(this.counterState);
      }
    }
  },
  mounted() {
    // this.getCounter();
    this.startCounter();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  height: 100vh;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
