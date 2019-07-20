<template>
  <div id="linechart">
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
      reISO: '/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2}):(\d{2}(?:\.\d*))(?:Z|(\+|-)([\d|:]*))?$/',
    }
  },
  methods: {

  },
  mounted() {
    var margin = {top: 50, right: 50, bottom: 50, left: 50}
    var width = window.innerWidth - margin.left - margin.right
    var height = window.innerHeight - margin.top - margin.bottom
    const numberOfDaysWeek = 7;
    const numberOfDaysMon = 30;

    var dataGroup = d3.select("#linechart")
      .append("svg")
      .attr("width", width + margin)
      .attr("height", height + 2 * margin)
      .append("g")
      .attr("transform", "translate(" + margin + "," + margin + ")");

    var parseTime = d3.timeParse("%Y-%m-%dT%H:%M:%SZ");

    d3.json('responsetime.json')
      .then(function(data) {

        var d = data["metrics"];

        var line = d3.line()
          .x(d => x(d.date))
          .y(d => y(d.averageResponseTime))
          ;

        d.forEach(function (d) {
          d.date = parseTime(d.date);
        })

        var x = d3.scaleTime()
          .domain(d3.extent(d, function (i) { return i.date; }))
          .range([0, width])
          ;
        
        var y = d3.scaleLinear()
          .domain(d3.extent(d, function (i) { return i.averageResponseTime; }))
          .range([height, 0])
          ;

        dataGroup.append("path")
          .data([d])
          .attr("fill", "none")
          .attr("stroke", "red")
          .attr("d", line)

        // var strictIsoParse = d3.utcParse("%Y-%m-%dT%H:%M:%S.%LZ");
        // if (typeof strictIsoParse != 'string')
        //     console.log(strictIsoParse + " not a string");
        // else 
        //     console.log(strictIsoParse + " is a string");
        // if (typeof d != 'string')
        //     console.log(d + " is date");
        // else 
        //     console.log(d + " is a string");

        // const now = new Date();
        // var day = now.getDate();
        // var month = (now.getMonth()+1);
        // console.log("month: "+month+" day: "+day);

        // var scale = d3.scaleLinear()
        //   .domain([0, 10]).range([0, 400]);
        // var xAxis = d3.axisBottom().scale(scale);
        //   d3.select('x-axis').call(xAxis);

        // var line = d3.line()
        //   .x(function(d) {return x(d.date); })
        //   .y(function(d) {return y(d.averageResponseTime); })
        //   .curve()
        //   ;
        
        // console.log(d.averageResponseTime)

        // var x = d3.scaleTime().range([0, width]);
        // x.domain(d3.extent(line, function(d) {return d.date}));
    
        // var y = d3.scaleLinear().range([height, 0]);
        // x.domain([d3.min(line, function(d) {return d.averageResponseTime })-5, 100]);

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

        // dataGroup.append("path")
        //   .data([data["metrics"]])
        //   .attr("fill", "none")
        //   .attr("stroke", "red")
        //   .attr("d", line)

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