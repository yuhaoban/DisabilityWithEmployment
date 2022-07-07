<template>
<div id='membercontainer' class='d-flex flex-column align-items-center justify-content-center min-vh-100'>
  <div id="member" class='text-center'>
    <b-row>
      <b-col><MemberInfoCom :memberInfo="Hao"/></b-col>
      <b-col><MemberInfoCom :memberInfo="Yuhao"/></b-col>
      <b-col><MemberInfoCom :memberInfo="Stefan"/></b-col>
      <b-col><MemberInfoCom :memberInfo="Emily"/></b-col>
    </b-row>
  </div>
</div>
</template>

<script>
import MemberInfoCom from './MemberInfo'
export default {
  name: 'member',
  components: { MemberInfoCom },
  props: {
  },
  data () {
    return {
      Hao: {
        name: 'Hao Yang',
        github: 'https://github.com/hyang800',
        img: 'https://avatars0.githubusercontent.com/u/49030962?s=460&v=4'
      },
      Yuhao: {
        name: 'Yuhao Ban',
        github: 'https://github.com/yuhaoban',
        img: 'https://avatars.githubusercontent.com/u/47099810?v=4'
      },
      Stefan: {
        name: 'Stefan(Yujie) Lin',
        github: 'https://github.com/StefanEMLin',
        img: 'https://avatars.githubusercontent.com/u/77991883?v=4'
      },
      Emily: {
        name: 'Emily Christiansen',
        github: 'https://github.com/emily-christiansen',
        img: 'https://avatars.githubusercontent.com/u/89543996?v=4'
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#membercontainer{
  width:100%;
  height:100vh;
  background-color: rgb(33, 43, 38);
  opacity: 0.9;
}
#member {
  padding-top: 80px;
  color:black;
}
</style>
