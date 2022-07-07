<template>
  <div class="home">
    <HelloWorld/>
    <Overview/>
    <D3map/>
    <Piechart/>
    <Circlep/>
    <Barchart/>
    <Linechart/>
    <Mapbox/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import Piechart from '@/views/Piechart.vue'
import Circlep from '@/views/circle_packing.vue'
import Barchart from '@/views/Barchart.vue'
import Mapbox from '@/views/Mapbox.vue'
import D3map from '@/views/D3map.vue'
import Linechart from '@/views/Linechart.vue'
import Overview from '@/views/Overview.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld,
    Piechart,
    Circlep,
    Barchart,
    Mapbox,
    D3map,
    Linechart,
    Overview
  }
}
</script>
