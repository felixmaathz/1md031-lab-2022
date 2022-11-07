

<template>
  <div class="wrapper">
    <div>
      {{ burger.name }} {{ burger.kCal }}
    </div>

    <div class="box a">
      <h4>{{ burger.productName }}</h4>
      <img v-bind:src="burger.imgURL" style="width: 200px;height: 130px">
      <ul style="text-align: left">
        <li>{{burger.kcal+"kcal"}}</li>
        <li class="lactose">Contains lactose</li>
        <li class="gluten">Contains gluten</li>
      </ul>
    </div>
  </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
  </style>
  