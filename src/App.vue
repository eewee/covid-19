<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col text-center">
        <a href="https://www.youtube.com/watch?v=cUSfL6MBmlY" target="_blank">YouTube</a> |
        <a href="https://codesandbox.io/s/vue-chartjs-demo-t1ps9?file=/src/components/AreaChart.vue" target="_blank">CodeSandbox.io</a>
      </div>
    </div>

    <div class="row mt-5" v-if="arrPositive.length > 0">
      <div class="col">
        <h2>Covid 19</h2>
        <line-chart
            :chartData="arrPositive"
            :options="chartOptions"
            label="Positive">
        </line-chart>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
import LineChart from "./components/LineChart";

export default {
  name: 'App',
  components: {
    LineChart
  },
  data() {
    return {
      arrPositive: [],
      arrHopitalized: [],
      arrInIcu: [],
      arrOnVentilators: [],
      arrRecevered: [],
      arrDeaths: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }
    }
  },
  async created() {
    const { data } = await axios.get("https://covidtracking.com/api/us/daily")
    data.forEach(d => {
      const date = moment(d.date, 'YYYYMMDD').format("MM/DD");

      const {
        positive,
        hospitalizedCumulative,
        inIcuCurrently,
        onVentilatorCurrently,
        recovered,
        death
      } = d;

      this.arrPositive.push({date, total: positive})
      this.arrHopitalized.push({date, total: hospitalizedCumulative})
      this.arrInIcu.push({date, total: inIcuCurrently})
      this.arrOnVentilators.push({date, total: onVentilatorCurrently})
      this.arrRecevered.push({date, total: recovered})
      this.arrDeaths.push({date, total: death})
    })
  }
}
</script>

<style>
body {
  color: #333;
}
</style>
