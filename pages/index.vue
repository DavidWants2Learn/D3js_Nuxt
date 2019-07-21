<template>
  <div id="bartarget">
    <div>
      <horizontalBarChart />
    </div>
    <div>
      <render />
    </div>
    <div>
      <lineChart />
    </div>
    <!-- {{transactionData}} -->
</div>
</template>

<script>
import * as d3 from 'd3';

import axios from 'axios'
// import transactionData from '~/assets/data/transaction.json'
import render from '~/components/render.vue'
import lineChart from '~/components/lineChart.vue'
import horizontalBarChart from '~/components/horizontalBarChart.vue'

export default {
  components: {
    render,
    lineChart,
    horizontalBarChart
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
