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
      margin: 0,
      width: 0,
      height: 0
    }
  },
  methods: {

  },
  mounted() {
    var margin = {top: 50, right: 50, bottom: 50, left: 50}
    var width = window.innerWidth - margin.left - margin.right
    var height = window.innerHeight - margin.top - margin.bottom

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
        var d = data["metrics"][0];
        console.log("You are here");
        // console.log(d);
        // console.log(d.date);
        // console.log(d[0].date);
        // console.log(d[0].averageResponseTime);

        var line = d3.line()
          .x(function(d) {return x(d.date); })
          .y(function(d) {return y(d.averageResponseTime); })
          .curve()
          ;

        console.log(d.averageResponseTime)

        var x = d3.scaleTime().range([0, width]);
        x.domain(d3.extent(line, function(d) {return d.date}));
    
        var y = d3.scaleLinear().range([height, 0]);
        x.domain([d3.min(line, function(d) {return d.averageResponseTime })-5, 100]);

        // var x = d3.scaleTime()
        //     .domain(d3.extent(data, function (d) {return d.date; }))
        //     .range([0, width])
        //     ;
        // // console.log(x);

        // var y = d3.scaleLinear()
        //     .domain(d3.extent(data, function (d) {return d.value; }))
        //     .range([0, height])
        //     ;
        // console.log(y);

        dataGroup.append("path")
          .data([data["metrics"]])
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

.line {
    fill: none;
    stroke: #ffab00;
    stroke-width: 3;
}

/* Style the dots by assigning a fill and stroke */
.dot {
    fill: #ffab00;
    stroke: #fff;
}

</style>