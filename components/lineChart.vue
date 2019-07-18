<template>
  <div class="linechart">
    <p>Line Chart</p>
  </div>
</template>

<script>
import * as d3 from 'd3';
import transactionData from '../static/t.json'

export default {
  name: 'Line-Chart',
  asyncData({ params }) {
    return {
      transactionData
    }
  },
  data() {
    return {
      tData: transactionData,
    }
  },
  methods: {

  },
  mounted() {

  }
}
</script>

<style>
</style>