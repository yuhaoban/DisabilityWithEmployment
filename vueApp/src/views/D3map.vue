<template>
<div class="d-flex align-items-center min-vh-100" id='d3map'>
  <svg id="d3mapsvg"></svg>
  <div class = 'col-sm-4'>
    <h1 class='text-center'>Domestic Distribution</h1>
    <div class="commands">
    <input type="button" class="filter" id="Total"  value="Total">
    <input type="button" class="filter" id="val15" value="2015">
    <input type="button" class="filter" id="val16" value="2016">
    <input type="button" class="filter" id="val17" value="2017">
    <input type="button" class="filter" id="val18" value="2018">
    <input type="button" class="filter" id="val19" value="2019">
    </div>
  </div>
</div>
</template>

<script>
import * as d3 from 'd3v7'
import * as topojson from 'topojson'

export default {
  name: 'Mapbox',
  mounted () {
    /// //////////////////////////////////////////////////////////
    // Choropleth code

    var promises = []

    var total, val15, val16, val17, val18, val19
    // var states
    var us
    var current
    var color

    promises.push(d3.json('states-albers-10m.json'))
    promises.push(d3.csv('number.csv'))

    Promise.all(promises).then(function (values) {
      // console.log(values)
      us = values[0]
      var data = values[1]
      total = d3
        .rollups(
          data,
          xs => d3.sum(xs, x => x.rate),
          d => d.id
        )
        .map(([k, v]) => ({ id: k, rate: v }))
      // console.log(total)
      val15 = data.filter(d => d.year === '2015')
      // console.log(val15)
      val16 = data.filter(d => d.year === '2016')
      val17 = data.filter(d => d.year === '2017')
      val18 = data.filter(d => d.year === '2018')
      val19 = data.filter(d => d.year === '2019')

      // this.states = new Map(us.objects.states.geometries.map(d => [d.id, d.properties]))
      var format = d => `${d}`

      color = d3.scaleQuantize([23751, 71107, 182709, 303196, 1567646], d3.schemeBlues[5])
      // console.log(color)

      data = Object.assign(new Map(total.map((d) => [d.id, +d.rate])))
      // data.title = ;
      // console.log(data)

      var path = d3.geoPath()

      var svg = d3.select('#d3mapsvg')
        .attr('viewBox', [0, 0, 975, 610])

      svg.append('g')
        .attr('transform', 'translate(610,20)')
        .append(() => legend({ color, title: data.title, width: 260 }))

      svg.append('g')
        .selectAll('path')
        .data(topojson.feature(us, us.objects.states).features)
        .join('path')
        .attr('fill', d => color(data.get(d.id)))
        .attr('d', path)
        .append('title')
        .text(d => `${d.properties.name}, ${format(data.get(d.id))}`)

      svg.append('path')
        .datum(topojson.mesh(us, us.objects.states, (a, b) => a !== b))
        .attr('fill', 'none')
        .attr('stroke', 'white')
        .attr('stroke-linejoin', 'round')
        .attr('d', path)
    })

    d3.select('#Total')
      .on('click', () => {
        current = total
        // console.log(current)
        // console.log(d3.extent(current.map(d => parseInt(d.rate))))
        redraw([23751, 71107, 182709, 303196, 1567646], d3.schemeBlues[5])
      })
    d3.select('#val15')
      .on('click', () => {
        current = val15
        // console.log(d3.extent(current.map(d => parseInt(d.rate))))
        redraw([4728, 13956.5, 36560, 58851.5, 13000, 26000, 309636], d3.schemeGreens[5])
      })
    d3.select('#val16')
      .on('click', () => {
        current = val16
        // console.log(d3.extent(current.map(d => parseInt(d.rate))))
        redraw([4717, 13893.5, 36367, 59399, 311169], d3.schemeOranges[5])
      })

    d3.select('#val17')
      .on('click', () => {
        //   filt('#2016');
        current = val17
        // console.log(d3.extent(current.map(d => parseInt(d.rate))))
        redraw([4682, 14139.5, 36639, 60569, 312895], d3.schemePurples[5])
      })

    d3.select('#val18')
      .on('click', () => {
        //   filt('#2016');
        current = val18
        // console.log(d3.extent(current.map(d => parseInt(d.rate))))
        redraw([4748, 14427, 36373, 61568, 315731], d3.schemeReds[5])
      })

    d3.select('#val19')
      .on('click', () => {
        //   filt('#2016');
        current = val19
        // console.log(d3.extent(current.map(d => parseInt(d.rate))))
        redraw([4876, 14690.5, 36770, 62808.5, 318215], d3.schemeRdPu[5])
      })

    function redraw (interval, col) {
      var data = Object.assign(new Map(current.map((d) => [d.id, +d.rate])))

      var path = d3.geoPath()

      var svg = d3.select('#d3mapsvg')
        .attr('viewBox', [0, 0, 975, 610])

      color = d3.scaleQuantize(interval, col)

      svg.append('g')
        .attr('transform', 'translate(610,20)')
        .append(() => legend({ color, title: data.title, width: 260 }))

      svg.append('g')
        .selectAll('path')
        .data(topojson.feature(us, us.objects.states).features)
        .join('path')
        .attr('fill', d => color(data.get(d.id)))
        .attr('d', path)
        .append('title')
        .text(d => `${d.properties.name}, ${d3.format(data.get(d.id))}`)

      svg.append('path')
        .datum(topojson.mesh(us, us.objects.states, (a, b) => a !== b))
        .attr('fill', 'none')
        .attr('stroke', 'white')
        .attr('stroke-linejoin', 'round')
        .attr('d', path)
    }

    function legend ({
      color,
      title = 'Total numbers of people involved in the survey',
      tickSize = 6,
      width = 320,
      height = 44 + tickSize,
      marginTop = 18,
      marginRight = 0,
      marginBottom = 16 + tickSize,
      marginLeft = 0,
      ticks = width / 64,
      tickFormat,
      tickValues
    } = {}) {
      if (document.getElementById('legend')) {
        d3.select('#legend').remove()
      }
      var svg = d3.create('svg')
        .attr('id', 'legend')
        .attr('width', width)
        .attr('height', height)
        .attr('viewBox', [0, 0, width, height])
        .style('overflow', 'visible')
        .style('display', 'block')
      let x

      // Continuous
      if (color.interpolator) {
        x = Object.assign(color.copy()
          .interpolator(d3.interpolateRound(marginLeft, width - marginRight)),
        { range () { return [marginLeft, width - marginRight] } })

        svg.append('image')
          .attr('x', marginLeft)
          .attr('y', marginTop)
          .attr('width', width - marginLeft - marginRight)
          .attr('height', height - marginTop - marginBottom)
          .attr('preserveAspectRatio', 'none')
          .attr('xlink:href', ramp(color.interpolator()).toDataURL())

        // scaleSequentialQuantile doesn’t implement ticks or tickFormat.
        if (!x.ticks) {
          if (tickValues === undefined) {
            const n = Math.round(ticks + 1)
            tickValues = d3.range(n).map(i => d3.quantile(color.domain(), i / (n - 1)))
          }
          if (typeof tickFormat !== 'function') {
            tickFormat = d3.format(tickFormat === undefined ? ',f' : tickFormat)
          }
        }
      } else if (color.invertExtent) {
        const thresholds =
            color.thresholds ? color.thresholds() // scaleQuantize
              : color.quantiles ? color.quantiles() // scaleQuantile
                : color.domain() // scaleThreshold

        const thresholdFormat =
            tickFormat === undefined ? d => d
              : typeof tickFormat === 'string' ? d3.format(tickFormat)
                : tickFormat

        x = d3.scaleLinear()
          .domain([-1, color.range().length - 1])
          .rangeRound([marginLeft, width - marginRight])

        svg.append('g')
          .selectAll('rect')
          .data(color.range())
          .join('rect')
          .attr('x', (d, i) => x(i - 1))
          .attr('y', marginTop)
          .attr('width', (d, i) => x(i) - x(i - 1))
          .attr('height', height - marginTop - marginBottom)
          .attr('fill', d => d)

        tickValues = d3.range(thresholds.length)
        tickFormat = i => thresholdFormat(thresholds[i], i)
      }

      svg.append('g')
        .attr('transform', `translate(0, ${height - marginBottom})`)
        .call(d3.axisBottom(x)
          .ticks(ticks, typeof tickFormat === 'string' ? tickFormat : undefined)
          .tickFormat(typeof tickFormat === 'function' ? tickFormat : undefined)
          .tickSize(tickSize)
          .tickValues(tickValues))
        .call(g => g.selectAll('.tick line').attr('y1', marginTop + marginBottom - height))
        .call(g => g.select('.domain').remove())
        .call(g => g.append('text')
          .attr('y', marginTop + marginBottom - height - 6)
          .attr('fill', 'currentColor')
          .attr('text-anchor', 'start')
          .attr('font-weight', 'bold')
          .text(title))

      return svg.node()
    }

    function ramp (color, n = 256) {
      const canvas = d3.DOM.canvas(n, 1)
      const context = canvas.getContext('2d')
      for (let i = 0; i < n; ++i) {
        context.fillStyle = color(i / (n - 1))
        context.fillRect(i, 0, 1, 1)
      }
      return canvas
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap');
#d3map {
    position: relative;
    padding: 0px;
    background-color: rgb(33, 43, 38);
    opacity: 0.9;
}

#d3map h1{
  font-size:80px;
}
#d3map svg{
  position: relative;
  width:65%;
  height:65%;
}
.commands{
  position: relative;
}
.filter{
  background-color: rgb(33, 43, 38);
  border: 2px solid #E6AC9E;
  color: #E6AC9E;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-family: 'Roboto', sans-serif;
  font-weight: 800;
  font-size: 20px;
  margin: 50px 13px;
  cursor: pointer;
  border-radius: 30px;
}

input[type='button'].filter:hover{
  background-color: rgb(250, 235, 207);
  color: rgb(33, 43, 38);
  border: 2px solid rgb(33, 43, 38);
}

input[type='button'].filter:focus{
  background-color: rgb(250, 235, 207);
  color: rgb(33, 43, 38);
  border: 2px solid rgb(33, 43, 38);
}
</style>
