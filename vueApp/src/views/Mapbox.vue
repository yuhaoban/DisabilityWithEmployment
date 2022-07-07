<template>
<div id = 'mapbox' class ='d-flex flex-row align-items-center min-vh-100'>
  <div class='col-sm-6'>
    <div id='map2' class='mt-5'></div>
    <div class='map-overlay' id='features'>
    </div>
  </div>
  <div class='col-sm-4 offset-sm-1'>
    <h1>Income by State</h1>
    <p class='text-left'>Annual number of people with disability who involved in the census by state from 2015 to 2019</p>
    <div class='map-overlay' id='kk'>
      <h2>The average income of disability people in each state</h2>
      <div id='pd'>
        <p>Hover over a state!</p>
      </div>
    </div>
  </div>
</div>

</template>

<script>
import mapboxgl from 'mapbox-gl'

export default {
  name: 'Mapbox',
  mounted () {
    this.createMap()
  },
  methods: {
    createMap () {
      mapboxgl.accessToken = 'pk.eyJ1IjoieXVoYW9iYW4xMjMiLCJhIjoiY2t2MDIwMmllMnZneDJ3cTlpN3U3ejY5byJ9.zXo1W4E1TcbB9GZAzHOogQ'
      this.map2 = new mapboxgl.Map({
        container: 'map2', // container ID
        style: 'mapbox://styles/yuhaoban123/ckwa3zcf8c02615rzzlnqcmq0', // style URL
        center: [-99.771556, 40.967243], // starting position [lng, lat]
        zoom: 3.0 // starting zoom
      })
      // console.log(this.map2)
      this.map2.on('load', function () {
      // map2.addSource('footprints_source', {
      //  type: 'geojson',
      //  data: 'mapbox.geojson'
      // })

        const layers = [
          '0-29906',
          '29907-36054',
          '36055-38844',
          '38845-43285',
          '43286-62500',
          '62500+'
        ]
        const colors = [
          '#ffffcc',
          '#c7e9b4',
          '#7fcdbb',
          '#41b6c4',
          '#2c7fb8',
          '#253494'
        ]

        const legend = document.getElementById('features')
        layers.forEach((layer, i) => {
          const color = colors[i]
          const item = document.createElement('div')
          const key = document.createElement('span')
          key.className = 'legend-key'
          key.style.backgroundColor = color
          const value = document.createElement('span')
          value.innerHTML = `${layer}`
          item.appendChild(key)
          item.appendChild(value)
          legend.appendChild(item)
        })
      })
      this.map2.on('mousemove', (event) => {
        const states = this.map2.queryRenderedFeatures(event.point, {
          layers: ['incdata']
        })
        document.getElementById('pd').innerHTML = states.length
          ? `<h3>${states[0].properties.name}</h3><p><strong><em>$${states[0].properties.avg_income.toFixed(2)}</strong> average income</em></p>`
          : '<p>Hover over a state!</p>'
      })
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@100;200;300;400;500;600;700;800;900&family=Poppins:wght@100;200;300;400;500;600;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Ubuntu:wght@400;500;700&display=swap');
#mapbox {
    background-color: rgb(33, 43, 38);
    opacity: 0.9;
    position: relative;
    display: inline-block;
}

#mapbox h1{
  font-size:60px;
}

#mapbox p{
  font-size: 28px;
}
#map2 {
    position: relative;
    height:500px;
    width: 800px;
    top:140px;
}

.legend-key {
    display: inline-block;
    border-radius: 20%;
    width: 10px;
    height: 10px;
    margin-right: 5px;
    text-align: left;
}

.map-overlay {
    bottom: 0;
    left: 18%;
    background: #fff;
    margin-right: 20px;
    font-size: 12px;
    font-family: 'Outfit', sans-serif;
    overflow: auto;
    border-radius: 3px;
    text-align: center;
}

.map-overlay h2{
  font-size: 28px;
}

#pd p{
  font-size: 20px;
}
#features {
  padding: 10px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  line-height: 18px;
  height: 138px;
  margin-bottom: 40px;
  width: 140px;
  border: none;
  border-radius: 40px;
  color:black;
  /*background-color: rgb(33, 43, 38);*/
  position: relative;
  left:660px;
  top:-19px;
}

#features span{
  font-family: 'Outfit', sans-serif;
  vertical-align: middle;
}
.title2{
  font-size:12px;
  font-family: 'Outfit', sans-serif;
  font-weight:500;
}
#kk {
    padding: 10px;
    margin-top: 20px;
    width: 300px;
    border: 2px solid #E6AC9E;
    border-radius: 40px;
    background-color: rgb(33, 43, 38);
}
</style>
