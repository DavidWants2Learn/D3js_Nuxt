<template>
  <div class="linechart">
    <p>LineChart</p>
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
      margin: 0,
      width: 0,
      height: 0,
    }
  },
  methods: {

  },
  mounted() {
    var margin = {top: 50, right: 50, bottom: 50, left: 50};
    var width = window.innerWidth - margin.left - margin.right;
    var height = window.innerHeight - margin.top - margin.bottom;
    const numberOfDaysWeek = 7;
    const numberOfDaysMon = 30;
    var d = [];

    var dataGroup = d3.select(".linechart")
      .append("svg")
      .attr("width", width)
      //  + margin)
      .attr("height", height)
      //  + 2 * margin)
      .append("g")
      .attr("transform", "translate(" + margin + "," + margin + ")");

    var parseTime = d3.timeParse("%Y-%m-%dT%H:%M:%SZ");

    d3.json('responsetime.json')
      .then(function(data) {
        // console.log(data["metrics"][0])

        for (var i = 0; i<numberOfDaysWeek; i++) {
          d[i] = data["metrics"][i];
          d[i].date = parseTime(d[i].date)
        }

        var line = d3.line()
          .x(d => x(d.date))
          .y(d => y(d.averageResponseTime))
          ;

        // d.forEach(function (d) {
        //   d.date = parseTime(d.date);
        // })

        var x = d3.scaleTime()
          .domain(d3.extent(d, function (i) { return i.date; }))
          .range([0, width - margin.right], .1)
          ;
        
        var y = d3.scaleLinear()
          .domain(d3.extent(d, function (i) { return i.averageResponseTime; }))
          .range([height, 0])
          ;

        dataGroup.append("path")
          .data([d])
          .attr("fill", "none")
          .attr("stroke", "blue")
          .attr("d", line)

        var xAxisGroup = dataGroup
          .append("g")
          .attr("class", "xAxisGroup")
          .attr("transform", "translate(0," + height + ")")

        var xAxis = d3.axisBottom(x)
          .tickFormat(d3.timeFormat("%m-%d"))
          // .tickPadding(10)
          ;

        xAxis(xAxisGroup);

        var yAxisGroup = dataGroup
          .append("g")
          .attr("class", "yAxisGroup")

        var yAxis = d3.axisRight(y)
          .ticks(10)
          ;

        yAxis(yAxisGroup);

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
    width: 1000px;
    height: 1000px;
    padding: 50px;
}

.yAxisGroup {
  padding: 100px;
}

.line {
    fill: none;
    stroke: #ffab00;
    stroke-width: 3;
}
</style>