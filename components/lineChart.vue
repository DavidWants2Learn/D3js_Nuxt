<template>
  <div class="linechart">
    <p>Line Chart</p>
    <svg>
        
    </svg>
  </div>
</template>

<script>
import * as d3 from 'd3';
import responseTime from '../static/responsetime.json'

export default {
  name: 'Line-Chart',
  asyncData({ params }) {
    return {
      responseTime
    }
  },
  data() {
    return {
      tData: responseTime,
      margin: 50,
      width: 1024,
      height: 768
    }
  },
  methods: {

  },
  mounted() {
    var margin = this.margin
    var width = this.width
    var height = this.height

    var dataGroup = d3.select("linechart")
      .attr("width", width + margin)
      .attr("height", height + 2 * margin)
      .append("g")
      .attr("transform", "translate(" + margin + ",")
    
    d3.json('responsetime.json')
      .then(function(data) {
        // console.log("Loading data");
        // console.log(data["metrics"][0]);
        // console.log("data loaded");
        var d = data["metrics"];
        console.log(d);
        // console.log(d[0].date);
        // console.log(d[0].averageResponseTime);
        var line = d3.line()
          .x(d => x(d.date))
          .y(d => y(d.averageResponseTime))
          ;
        
        // Attempting to make date time standard
        // var parseTime = d3.timeParse("%m/%d/%Y");
        // data["metrics"].forEach(function (d) {
        //   console.log(d.date)
        //   d.date = parseTime(d.date);
        //   console.log(d.date)
        // });

        var x = d3.scaleTime()
            .domain(d3.extent(data, function (d) {return d.date; }))
            .range([0, width])
            ;

        var y = d3.scaleLinear()
            .domain(d3.extent(data, function (d) {return d.value; }))
            .range([0, height])
            ;

        dataGroup.append("path")
            .data([data])
            .attr("fill", "none")
            .attr("stroke", "red")
            .attr("d", line)

      }, function(error) {
        console.log("error loading data");
        console.log(error);
      });
  }
}
</script>

<style>
  svg {
    border: 1px solid grey;
}
</style>