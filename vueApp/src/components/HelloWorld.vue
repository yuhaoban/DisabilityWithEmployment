<template>
<div>
  <div id='container1' class="d-flex flex-column align-items-center min-vh-100">
      <h1 class='col-sm-12 text-center'><br>Project Crushers</h1>
      <img class='col-sm-12' src='../assets/d-11.png'>
  </div>
</div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@100;200;300;400;500;600;700;800;900&family=Poppins:wght@100;200;300;400;500;600;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Ubuntu:wght@400;500;700&display=swap');
#container1{
  /*background-image: linear-gradient(rgba(0, 0, 0, 0.192),rgba(0, 0, 0, 0.068)),url('../assets/d-3.jpg');*/
  background-color: rgb(33, 43, 38);
  opacity: 0.9;
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

#container1 h1{
  color: rgb(250, 235, 207);
  font-family: "Outfit",sans-serif;
  font-weight:800;
  font-size:8rem;
  animation: moveright 1s linear 1;
  animation-delay: 3s;
  visibility: hidden;
  animation-fill-mode: forwards;
}

@keyframes moveright{
  0%{
    transform: translateX(-800px);
    visibility: visible;
  }
  100%{
    transform: translateX(0px);
    visibility: visible;
  }
}

#container1 img{
  animation: zoomin 1s linear 1;
  animation-delay: 4s;
  visibility: hidden;
  animation-fill-mode: forwards;
}

@keyframes zoomin{
  0%{
    transform: scale(0.5);
    visibility: visible;
  }
  100%{
    transform: scale(1.0);
    visibility: visible;
  }
}

#container1 h2{
  color: rgb(250, 235, 207);
  font-family: "Ubuntu",sans-serif;
  font-size: 30px;
  font-weight: 700;
}
</style>
