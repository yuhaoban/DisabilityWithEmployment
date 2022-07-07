<template>
  <div class='d-flex flex-row align-items-center min-vh-100' id='circlep'>
      <div class='col-sm-6'>
        <svg id="chart2" height='800px'></svg>
      </div>
      <div class='col-sm-4 offset-sm-1'>
        <h1>Population Ratios By Disability Type
        <br><br>
        <p>For each of the five major measured disability categories, zoom to compare the total
           counts of individuals that suffer from these disabilities in different sections of the
           labor force.</p></h1>
      </div>
  </div>
</template>

<script>
import * as d3 from 'd3'
import axios from 'axios'

export default {
  name: 'circiep',
  mounted () {
    axios.get('flare-2.json').then(result => {
      var data = result.data
      // console.log(data)

      var width = 700
      var height = width

      var pack = d3.pack()
        .size([width, height])
        .padding(3)

      var root = d3.hierarchy(data)
        .sum(d => d.value)
        .sort((a, b) => b.value - a.value)

      var focus = pack(root)
      // console.log(focus)
      const nodes = pack(root).descendants()
      // console.log(nodes)
      var view

      const svg = d3.select('#chart2')
        .attr('viewBox', `-${width / 2} -${height / 2} ${width} ${height}`)
        .style('display', 'block')
        // .style('margin', '0 -14px')
        .style('background', 'rgb(33, 43, 38)')
        .style('cursor', 'pointer')
        .on('click', (event) => zoom(root))

      const node = svg.append('g')
        .selectAll('circle')
        .data(nodes.slice(1))
        .enter().append('circle')
        .attr('fill', d => d.children ? '#58171e' : 'rgba(245,175,90,255)')
        .attr('pointer-events', d => !d.children ? 'none' : null)
        .on('mouseover', function () { d3.select(this).attr('stroke', 'rgba(245,175,90,255)') })
        .on('mouseout', function () { d3.select(this).attr('stroke', null) })
        .on('click', (event, d) => focus !== d && (zoom(event, d), d3.event.stopPropagation()))

      const label = svg.append('g')
        .style('font', '10px sans-serif')
        .attr('pointer-events', 'none')
        .attr('text-anchor', 'middle')
        .selectAll('text')
        .data(nodes)
        .enter().append('text')
        .style('fill-opacity', d => d.parent === root ? 1 : 0)
        .style('display', d => d.parent === root ? 'inline' : 'none')
        .text(d => d.data.name)

      zoomTo([root.x, root.y, root.r * 2 + 150])

      function zoomTo (v) {
        const k = width / v[2]
        view = v
        label.attr('transform', d => `translate(${(d.x - v[0]) * k},${(d.y - v[1]) * k})`)
        node.attr('transform', d => `translate(${(d.x - v[0]) * k},${(d.y - v[1]) * k})`)
        node.attr('r', d => d.r * k)
      }

      function zoom (d) {
        focus = d
        const transition = svg.transition()
          .duration(d3.event.altKey ? 7500 : 750)
          .tween('zoom', d => {
            const i = d3.interpolateZoom(view, [focus.x, focus.y, focus.r * 2 + 150])
            return t => zoomTo(i(t))
          })
        // console.log(transition)
        transition.selectAll('text')
          .filter(function (d) { return d.parent === focus || this.style.display === 'inline' })
          .style('fill-opacity', d => d.parent === focus ? 1 : 0)
          .on('start', function (d) { if (d.parent === focus) this.style.display = 'inline' })
          .on('end', function (d) { if (d.parent !== focus) this.style.display = 'none' })
      }
    })
  }
}
</script>

<style>
#circlep{
  background-color: rgb(33, 43, 38);
  opacity: 0.9;
  width: 100%;
  display:inline-block;
}

#circlep h1{
  font-size: 60px;
}

#circlep p{
  font-size: 35px;
}

#chart2{
  position: relative;
}
.arc text {
  font: 10px sans-serif;
  text-anchor: middle;
}

.arc path {
  stroke: #fff;
}

#chart2 text{
  font-family: 'Roboto', sans-serif;
  font-weight: 800;
  fill: rgb(33, 43, 38);
  text-anchor: middle;
}
</style>
