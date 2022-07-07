<template>
  <div id="app">
    <b-navbar class='row align-items-center col-sm-12' id="nav" fixed='top'>
      <h4 class='col-sm-2 mt-1'>Crushers</h4>
      <router-link id='navrl' class='col-sm-1 offset-sm-6' style="text-decoration: none;" to="/member">Members </router-link>
      <a id='nava' class='col-sm-1' style="text-decoration: none;" href='https://github.com/DSCI-554/project-the-crashers' target="_blank">Github</a>
      <b-nav-item-dropdown id='dropdown' class='col-sm-1' text='Move to' toggle-class="nav-link-custom" aria-labelledby="dropdownMenuButton">
        <a class='dropdown-item' href='/#container1'>Home</a>
        <a class='dropdown-item' href='/#overview'>Overview</a>
        <a class='dropdown-item' href='/#d3map'>Domestic Distribution</a>
        <a class='dropdown-item' href='/#piechart'>Pie Chart</a>
        <a class='dropdown-item' href='/#circlep'>Circle Pakcing</a>
        <a class='dropdown-item' href='/#barchart'>Bar Chart</a>
        <a class='dropdown-item' href='/#linechart'>Line chart</a>
        <a class='dropdown-item' href='/#mapbox'>Mapbox Chart</a>
      </b-nav-item-dropdown>
    </b-navbar>
    <router-view/>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@100;200;300;400;500;600;700;800;900&family=Poppins:wght@100;200;300;400;500;600;700&display=swap');
#app {
  font-family: 'Outfit', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: rgb(250, 235, 207);
  overflow-x: hidden;
}
#nav {
  padding: 10px;
  background-color: rgba(33, 43, 38, 0.26);
  text-align: center;
}

#nav a {
  font-weight: 600;
  color: rgb(250, 235, 207);
}

#nav a.router-link-exact-active {
  color: #E6AC9E;
}

#nav h4{
  color: rgb(250, 235, 207);
  font-family: "Outfit",sans-serif;
  animation: movedown 1s linear 1;
  animation-delay: 1s;
  visibility: hidden;
  animation-fill-mode: forwards;
}

#navrl{
  font-size:19px;
  animation: movedown 1s linear 1;
  animation-delay: 2s;
  visibility: hidden;
  animation-fill-mode: forwards;
}

#nava{
  font-size:19px;
  animation: movedown 1s linear 1;
  animation-delay: 2s;
  visibility: hidden;
  animation-fill-mode: forwards;
}

@keyframes movedown{
  0%{
    transform: translateY(-100px);
    visibility: visible;
  }
  100%{
    transform: translateY(0px);
    visibility: visible;
  }
}

#dropdown{
  font-size:19px;
  list-style-type: none;
  text-align: center;
  animation: movedown 1s linear 1;
  animation-delay: 2s;
  visibility: hidden;
  animation-fill-mode: forwards;
}

.dropdown-menu.show{
  background-color: rgba(33, 43, 38, 0);
  left: -11px;
  border: 2px solid rgb(250, 235, 207);
}

#dropdown a{
  display: inline-block;
  text-decoration: none;
  text-align: center;
}

#dropdown a.dropdown-item {
  padding: 3px 10px;
}

#dropdown a.dropdown-item.hover, #dropdown a.dropdown-item:hover{
  background-color: rgb(250, 235, 207);
  color:rgb(33, 43, 38);
}

#dropdown a.dropdown-item.active, #dropdown a.dropdown-item:active{
  background-color: rgb(250, 179, 38);
  color:rgb(33, 43, 38);
}

</style>
