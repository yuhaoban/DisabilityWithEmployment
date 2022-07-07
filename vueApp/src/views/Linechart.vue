<template>
<div id='linechart' class='d-flex flex-row align-items-center min-vh-100'>
  <div class='col-sm-6 d-flex flex-column align-items-center'>
    <select id="selectButton"></select>
    <svg id="linechart2" width='100' height='50'></svg>
  </div>
  <div class='col-sm-4 offset-sm-1'>
    <h1>Average Income for Geographic Areas</h1>
    <br>
    <p class='text-left'>Explore and compare the average income values over the five years of data for individuals with and without disabilities as well as the average income for their geographical area.</p>
    <p class='text-left' style='color:#E5C494;'>Non-Disabled Average Income</p>
    <p class='text-left' style='color:#a6d854;'>Total Population Average Income</p>
    <p class='text-left' style='color:#ffd92f;'>Disabled Average Income</p>
  </div>
</div>
</template>

<script>
import * as d3 from 'd3'
import * as d3s from 'd3-scale-chromatic'

export default {
  name: 'linechart',
  mounted: function () {
    var xScale = d3.scaleTime()
    var yScale = d3.scaleLinear()
    var xAxis = d3.axisBottom()
      .scale(xScale)
    var yAxis = d3.axisLeft()
      .scale(yScale)
    // Read the data
    d3.csv('compare_.csv', function (data) {
      // set the dimensions and margins of the graph
      var margin = { top: 50, right: 50, bottom: 50, left: 100 }
      var width = parseInt(d3.select('#linechart2').style('width')) - margin.left - margin.right
      var height = parseInt(d3.select('#linechart2').style('height')) - margin.top - margin.bottom
      console.log(height)
      // width = 700 - margin.left - margin.right,
      // height = 400 - margin.top - margin.bottom;
      // append the svg object to the body of the page
      var svg = d3.select('#linechart2')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom)
        .append('g')
        .attr('transform',
          'translate(' + margin.left + ',' + margin.top + ')')
      var allGroup = d3.map(data, function (d) { return (d.area) }).keys()
      // add the options to the button
      d3.select('#selectButton')
        .selectAll('myOptions')
        .data(allGroup)
        .enter()
        .append('option')
        .text(function (d) { return d }) // text showed in the menu
        .attr('value', function (d) { return d }) // corresponding value returned by the button

      // A color scale: one color for each group
      var myColor = d3.scaleOrdinal()
        .domain(allGroup)
        .range(d3s.schemeSet2)

      xScale.range([0, width])
      yScale.range([height, 0])

      xScale.domain(d3.extent(data, function (d) { return d.year }))
      yScale.domain([15000, d3.max(data, function (d) { return +d.npwd_inc })])

      svg.append('g')
        .attr('class', 'x axis')
        .attr('transform', 'translate(0,' + (height) + ')')
        .call(xAxis.tickFormat(d3.format('d')).ticks(5))
        .style('stroke', 'rgb(250, 235, 207)')
        .selectAll('text')
        .style('text-anchor', 'end')
        .attr('dx', '-.8em')
        .attr('dy', '.15em')
        .attr('transform', 'rotate(-45)')

      svg.append('g')
        .attr('class', 'y axis')
        .call(yAxis)
        .style('stroke', 'rgb(250, 235, 207)')

      // Initialize line with first group of the list
      var line = svg
        .append('g')
        .append('path')
        .datum(data.filter(function (d) { return d.area === allGroup[0] }))
        .attr('d', d3.line()
          .x(function (d) { return xScale(d.year) })
          .y(function (d) { return yScale(+d.avg_inc) })
        )
        .attr('class', 'curve')
        .attr('id', 'line1')
        .attr('stroke', function (d) { return myColor('valueA') })
        .style('stroke-width', 4)
        .style('fill', 'none')

      var line2 = svg
        .append('g')
        .append('path')
        .datum(data.filter(function (d) { return d.area === allGroup[1] }))
        .attr('d', d3.line()
          .x(function (d) { return xScale(d.year) })
          .y(function (d) { return yScale(+d.pwd_inc) })
        )
        .attr('class', 'curve')
        .attr('id', 'line2')
        .attr('stroke', function (d) { return myColor('valueB') })
        .style('stroke-width', 4)
        .style('fill', 'none')

      var line3 = svg
        .append('g')
        .append('path')
        .datum(data.filter(function (d) { return d.area === allGroup[1] }))
        .attr('d', d3.line()
          .x(function (d) { return xScale(d.year) })
          .y(function (d) { return yScale(+d.npwd_inc) })
        )
        .attr('class', 'curve')
        .attr('id', 'line3')
        .attr('stroke', function (d) { return myColor('valueC') })
        .style('stroke-width', 4)
        .style('fill', 'none')

      // A function that update the chart
      function update (selectedGroup) {
        // Create new data with the selection?
        var dataFilter = data.filter(function (d) { return d.area === selectedGroup })

        // Give these new data to update line
        line
          .datum(dataFilter)
          .transition()
          .duration(1000)
          .attr('d', d3.line()
            .x(function (d) { return xScale(d.year) })
            .y(function (d) { return yScale(+d.avg_inc) })
          )
          .attr('stroke', function (d) { return myColor('valueA') })

        line2
          .datum(dataFilter)
          .transition()
          .duration(1000)
          .attr('d', d3.line()
            .x(function (d) { return xScale(d.year) })
            .y(function (d) { return yScale(+d.pwd_inc) })
          )
          .attr('stroke', function (d) { return myColor('valueB') })

        line3
          .datum(dataFilter)
          .transition()
          .duration(1000)
          .attr('d', d3.line()
            .x(function (d) { return xScale(d.year) })
            .y(function (d) { return yScale(+d.npwd_inc) })
          )
          .attr('stroke', function (d) { return myColor('valueC') })
      }

      // When the button is changed, run the updateChart function
      d3.select('#selectButton').on('change', function (d) {
        // recover the option that has been chosen
        var selectedOption = d3.select(this).property('value')
        // run the updateChart function with this selected option
        update(selectedOption)
      })
      resize()
      d3.select(window).on('resize', resize)
      function resize () {
        var width = parseInt(d3.select('#linechart2').style('width')) - margin.left - margin.right
        var height = parseInt(d3.select('#linechart2').style('height')) - margin.top - margin.bottom
        xScale.range([0, width])
        yScale.range([height, 0])
        yAxis.ticks(Math.max(height / 50, 2))
        xAxis.ticks(Math.max(width / 100, 2))
        svg.select('.x.axis')
          .attr('transform', 'translate(0,' + height + ')')
          .call(xAxis)
          .selectAll('text')
          .style('text-anchor', 'end')
          .attr('dx', '-.8em')
          .attr('dy', '.15em')
          .attr('transform', 'rotate(-45)')

        svg.select('.y.axis')
          .call(yAxis)
        var dataPerPixel = data.length / width
        var sample = data.filter(
          function (d, i) {
            return i % Math.ceil(dataPerPixel) === 0
          })

        svg.select('#line1')
          .datum(sample.filter(function (d) { return d.area === allGroup[0] }))
          .attr('d', d3.line()
            .x(function (d) { return xScale(d.year) })
            .y(function (d) { return yScale(+d.avg_inc) })
          )
          .attr('class', 'curve')
          .attr('id', 'line1')

        svg.select('#line2')
          .datum(sample.filter(function (d) { return d.area === allGroup[1] }))
          .attr('d', d3.line()
            .x(function (d) { return xScale(d.year) })
            .y(function (d) { return yScale(+d.pwd_inc) })
          )
          .attr('class', 'curve')
          .attr('id', 'line2')

        svg.select('#line3')
          .datum(sample.filter(function (d) { return d.area === allGroup[1] }))
          .attr('d', d3.line()
            .x(function (d) { return xScale(d.year) })
            .y(function (d) { return yScale(+d.npwd_inc) })
          )
          .attr('class', 'curve')
          .attr('id', 'line3')
      }
    })
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@100;200;300;400;500;600;700;800;900&family=Poppins:wght@100;200;300;400;500;600;700&display=swap');
#linechart{
  background-color: rgb(33, 43, 38);
  opacity: 0.9;
  display: inline-block;
}
#linechart h1{
  font-size: 50px;
}

#linechart p{
  font-size: 25px;
}
#linechart2{
  width:100%;
  height:100%;
}
#selectButton{
  background-color: rgb(33, 43, 38);
  border: 2px solid #E6AC9E;
  color: #E6AC9E;
  padding: 15px 30px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-family: 'Roboto', sans-serif;
  font-weight: 800;
  font-size: 16px;
  margin: 0px 0px;
  cursor: pointer;
  margin-top: 20px;
  border-radius: 30px;
}

#selectButton:focus{
  outline: 0;
}

.axis line {
  fill: rgb(250, 235, 207);
  stroke: rgb(250, 235, 207);
  shape-rendering: crispEdges;
}
.axis path{
  stroke: rgb(250, 235, 207);
}

.y.axis{
  font-family: 'Outfit', 'sans-serif';
  font-weight: 100;
  font-size: 15px;
}

.x.axis{
  font-family: 'Outfit', 'sans-serif';
  font-weight: 100;
  font-size: 15px;
}
</style>
