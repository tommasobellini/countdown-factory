<template>
  <CountDownWidget
    :timer="counter.counterString"
    :state="counterState"
  ></CountDownWidget>
</template>
<script>
import CountDownWidget from "./components/CountDownWidget.vue";
import axios from "axios";
import moment from "moment";
moment.locale("it");
const BASE_URL = "https://countdown-factory.herokuapp.com";
const GET_COUNTDOWN_API = BASE_URL + "/get_countdown/";
const DELETE_COUNTDOWN_API = BASE_URL + "/delete_countdown/";

export default {
  name: "App",
  components: {
    CountDownWidget,
  },
  data() {
    return {
      counter: {
        id: 0,
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
      setInterval(() => {
        this.tempCountDownTime = this.tempCountDownTime.subtract(1, "s");
        this.getCounter();
        this.getImage();
      }, 1000);
    },

    getCounterAPI(index) {
      axios.get(GET_COUNTDOWN_API).then((resp) => {
        var data = resp.data;
        this.counter.id = data[index].id;
        console.log("ID COUNTER API: "+this.counter.id);
        var tempEndDate = Date.parse(data[index].end_date);
        var x = moment(tempEndDate);
        var y = moment(Date.now());
        var duration = moment.duration(x.diff(y));
        this.tempCountDownTime = duration;
      });
    },

    deleteCounterAPI(){
      console.log("ID FROM DELETE: "+this.counter.id);
      axios.delete(DELETE_COUNTDOWN_API+"?id="+this.counter.id.toString()).then((resp)=> {
        console.log(resp);
        console.log(resp.data);
        if(resp.status >= 200 && resp.status<300){
          this.getCounterAPI(0);
        }
      });
    },

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

    getImage() {
      if (
        this.counter.days == 0 &&
        this.counter.hours == 0 &&
        this.counter.minutes == 0 &&
        this.counter.seconds == 0
      ) {
        if(!this.counterState){
        this.counterState = true;
        this.deleteCounterAPI();
        }
      } else {
        this.counterState = false;
      }
    },
  },

  mounted() {
    this.getCounterAPI(0);
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
