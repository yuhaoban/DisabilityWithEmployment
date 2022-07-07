<template>
<div id = 'overview' class='d-flex align-items-center min-vh-100'>
    <div class ='col-sm-3 offset-sm-1'>
      <img src = '../assets/icon-11.png'>
      </div>
      <div class='col-sm-6 offset-sm-1'>
        <h1 class='text-center'>Overview</h1>
        <h2><br>We are utilizing the 5-year IPUMS US census data from 2015 to 2019 to visualize the impact of a person’s disability status on their income level.
        <br>The goal is to raise the awareness of social equity issues in disability in the United States.<br>
        The infographic is to show how the social security and social institutions address this issue in different states over the past decade. The thesis is data driven so the visualization helps us to find proofs to understand the changes over time.
        </h2>
      </div>
</div>

</template>

<script>
export default {
  name: 'overview',
  mounted () {
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap');
#overview{
  background-color: rgb(33, 43, 38);
  opacity: 0.9;
}
#overview img{
  height:550px;
}

#overview h1{
  font-size: 150px;
}
#overview h2{
  font-size: 30px;
}
</style>
