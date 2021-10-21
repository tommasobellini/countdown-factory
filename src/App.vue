<template>
  <CountDownWidget :timer="counter.endDate"></CountDownWidget>
</template>
<script>
import CountDownWidget from "./components/CountDownWidget.vue";
import axios from "axios";
import moment from "moment";
const BASE_URL = " http://192.168.1.108:8000";
const COUNTDOWN_API = BASE_URL + "/get_countdown/";

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
        endDate: "",
               
      },
    tempEndDate: Date(), 

    };
  },
  methods: {
    startCounter() {
      setInterval(() => {
        this.tempEndDate= Date(this.tempEndDate-1);
        console.log( moment(this.tempEndDate).format('YYYY-MM-DD HH:mm:ss'));
        this.counter.endDate = moment(this.tempEndDate).format('YYYY-MM-DD HH:mm:ss');
      }, 1000);
    },
    getCounter(){
      axios.get(COUNTDOWN_API).then((resp) => {
        var data = resp.data;
        console.log(data.start_date);
        // var end_date =  moment(data.end_date, "dd-mm-YYYY HH:MM:SS");
        this.tempEndDate = Date.parse(data.endDate);
      });
    }
  },
  mounted() {
    this.getCounter();
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
