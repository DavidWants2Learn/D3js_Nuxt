<template>
  <div>
    <div class="renderclass">
      <p>Graph goes here</p>
    </div>
  </div>
</template>

<script>
import * as d3 from 'd3';
import transactionData from '../static/t.json'

export default {
  name: 'render',
  components: {
  },
  props: {
    // values: values
  },
  asyncData({ params }) {
    return {
      transactionData
    }
  },
  data() {
    return {
      tData: transactionData,
      values: [10,40,50,10,20,70,25],
      initialX: 0,
      deltaX: 25,
      initialY: 15,
      deltaY: 25
    }
  },
  methods: {
    drawCirc(radius) {
      var svg = d3.select(".renderclass")
      .append("svg")

      svg
        .append("circle")
        .attr("fill", "red")
        .attr("r", 20)
        .attr("cx", 50)
        .attr("cy", 50);
    },
    drawRect(gRect, radius, currentX, currenty) {
      gRect
        .append("rect")
        .attr("fill", "blue")
        .attr("width", 50) //bar width
        .attr("height", radius) //bar height
        .attr("x", currentX) //bargraph width 
        .attr("y", currenty); //height from top right
    }
  },
  created() {
    
  },
  mounted() {
    // console.log("MOUNTED");
    var values = this.values
    var currentX = this.initialX
    var currentY = this.initialY

    // console.log("RENDERING");
    var svg = d3.select(".renderclass")
      .append("svg")
      .attr("transform", "scale(0.5)")

    var drawRect = this.drawRect;
    // console.log(tData)
    // d3.json({transactionData}, function(error, data) {
    d3.json('t.json')
    .then(function(data) {
      //console.log(data);
      console.log(data["metrics"][0].numberOfTransactions);
      
      var gRect = svg.append("g")
      //values.forEach(x => drawRect(gRect, x, currentX += 55, currentY));
      //data["metrics"].forEach(x => this.drawRect(gRect, x.numberOfTransactions, currentX += 55, currentY));
      //data["metrics"].forEach(x => console.log(x.numberOfTransactions));
      data["metrics"].forEach(x => drawRect(gRect, (x.numberOfTransactions)/50000, currentX += 55, currentY));

    }, function(error) {
      console.log("JSON Data failed to upload.");
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