<template>
  <div>
    <div class="HorizontalBarChart">
      <p>Horizontal Graph</p>
    </div>
  </div>
</template>

<script>
import * as d3 from 'd3';
import statusData from '../static/status.json'

export default {
  name: 'render',
  components: {
  },
  props: {
    // values: values
  },
  asyncData({ params }) {
    return {
      statusData
    }
  },
  data() {
    return {
      sData: statusData,
    }
  },
  methods: {
    
  },
  created() {
    
  },
  mounted() {
    d3.json('status.json')
    .then(function(data) {
      console.log(data);
    }, function(error) {

    });
  }
}
</script>

<style>
  svg {
    width: 1000px;
    height: 1000px;
  }
</style>