<template>
<div id='barchart' class='d-flex flex-row align-items-center min-vh-100'>
  <div class='col-sm-6 d-flex flex-column align-items-center'>
    <div id='buttoncontainer' class='buttoncontainer mt-5'>
    <button class='buttonvar1' id='buttonvar1' @click="update('var1')">No Reported Disability</button>
    <button class='buttonvar2' id='buttonvar2' @click="update('var2')">Disabled</button>
    </div>
    <svg id='my_dataviz' height = "600px" width = "800px"></svg>
    <button id='b0' class='xbutton'>0</button>
    <button id='b1' class='xbutton'>10</button>
    <button id='b2' class='xbutton'>12</button>
    <button id='b3' class='xbutton'>14</button>
    <button id='b4' class='xbutton'>15</button>
    <button id='b5' class='xbutton'>20</button>
    <button id='b6' class='xbutton'>30</button>
  </div>
  <div class='col-sm-4 offset-sm-1'>
    <h1>Work Force Totals
    <br><br>
    <p class='text-left'>Compare the disabled employment statuses with values to those without a reported disability.
      <br>0 – N/A
      <br>10 – At Work
      <br>12 – Has job, not working
      <br>14 – Armed Forces, At Work
      <br>15 – Armed Forces, with job but not working
      <br>20 – Unemployed
      <br>30 – Not in labor force</p></h1>
  </div>
</div>
</template>

<script>
import * as d3 from 'd3'

export default {
  name: 'bar',
  mounted () {
    // set the dimensions and margins of the graph
    this.margin = { top: 30, right: 30, bottom: 50, left: 60 }
    this.width = 800 - this.margin.left - this.margin.right
    this.height = 600 - this.margin.top - this.margin.bottom

    // append the svg object to the body of the page
    this.svg = d3.select('#my_dataviz')
      .append('svg')
      .attr('width', this.width + this.margin.left + this.margin.right)
      .attr('height', this.height + this.margin.top + this.margin.bottom)
      .append('g')
      .attr('transform',
        'translate(' + this.margin.left + ',' + this.margin.top + ')')

    // Initialize the X axis
    this.x = d3.scaleBand()
      .range([0, this.width])
      .padding(0.2)

    // Initialize the Y axis
    this.y = d3.scaleLinear()
      .range([this.height, 0])
    // Initialize plot
    this.update('var1')
  },
  methods: {
    // A function that create / update the plot for a given variable:
    update (selectedVar) {
      // Parse the Data
      d3.csv('bar_data.csv', data => {
        // X axis
        this.x.domain(data.map(function (d) { return d.group }))

        // Add Y axis
        this.y.domain([0, d3.max(data, function (d) { return +d[selectedVar] })])

        // variable u: map data to existing bars
        var u = this.svg.selectAll('rect')
          .data(data)

        // console.log(this.x)
        // update bars
        u
          .enter()
          .append('rect')
          .merge(u)
          .transition()
          .duration(1000)
          .attr('x', d => { return this.x(d.group) })
          .attr('y', d => { return this.y(d[selectedVar]) })
          .attr('width', this.x.bandwidth())
          .attr('height', d => { return this.height - this.y(d[selectedVar]) })
          .attr('fill', '#E6AC9E')

        var y = this.svg.selectAll('text.rect')
          .data(data)
        // console.log(this.svg.selectAll('text'))
        // console.log(this.svg.selectAll('text')._groups[0].length)
        if (this.svg.selectAll('text')._groups[0].length !== 0) {
          this.svg.selectAll('text').remove()
        }
        y
          .enter()
          .append('text')
          .merge(y)
          .transition()
          .duration(1000)
          .attr('x', d => { return this.x(d.group) + 40 })
          .attr('y', d => { return this.y(d[selectedVar]) - 10 })
          .text(function (d) { return d[selectedVar] })
        // console.log(this.svg.selectAll('text'))
      })
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap');
#barchart{
  background-color: rgb(33, 43, 38);
  opacity: 0.9;
  height:100%;
  width:100%;
  display: inline-block;
}
#barchart h1{
  font-size:60px;
}

#barchart p{
  font-size: 30px;
}
#buttonvar1 {
  position: relative;
  background-color: rgb(33, 43, 38);
  border: 2px solid #E6AC9E;
  color: #E6AC9E;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-family: 'Roboto', sans-serif;
  font-weight: 800;
  font-size: 16px;
  margin: 40px 80px;
  cursor: pointer;
  border-radius: 30px;
}

#buttonvar2 {
  position: relative;
  background-color: rgb(33, 43, 38);
  border: 2px solid #E6AC9E;
  color: #E6AC9E;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-family: 'Roboto', sans-serif;
  font-size: 16px;
  font-weight: 800;
  margin: 20px 10px;
  cursor: pointer;
  border-radius: 30px;
}

#buttonvar1:hover{
  background-color: rgb(250, 235, 207);
  color: rgb(33, 43, 38);
  border: 2px solid rgb(33, 43, 38);
}

#buttonvar1:focus{
  background-color: rgb(250, 235, 207);
  color: rgb(33, 43, 38);
  border: 2px solid rgb(33, 43, 38);
}

#buttonvar2:hover{
  background-color: rgb(250, 235, 207);
  color: rgb(33, 43, 38);
  border: 2px solid rgb(33, 43, 38);
}

#buttonvar2:focus{
  background-color: rgb(250, 235, 207);
  color: rgb(33, 43, 38);
  border: 2px solid rgb(33, 43, 38);
}

.xbutton{
  background-color: rgb(33, 43, 38);
  border: 2px solid #E6AC9E;
  color: #E6AC9E;
  padding: 4px 30px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-family: 'Roboto', sans-serif;
  font-size: 16px;
  font-weight: 800;
  margin: 0px -10px;
  cursor: pointer;
  border-radius: 30px;
}

#b0{
  position: relative;
  top: -40px;
  left: -281px;
  padding: 4px 34px;
}

#b1{
  position: relative;
  top:-76px;
  left:-182px;
}

#b2{
  position: relative;
  top:-112px;
  left:-83px;
}

#b3{
  position: relative;
  top:-148px;
  left:16px;
}

#b4{
  position: relative;
  top:-184px;
  left:114px;
}

#b5{
  position: relative;
  top:-220px;
  left:213px;
}

#b6{
  position: relative;
  top:-256px;
  left:312px;
}

#my_dataviz text{
  font-family: 'Roboto', sans-serif;
  font-weight: 800;
  fill: #E6AC9E;
  text-anchor: middle;
}
</style>
