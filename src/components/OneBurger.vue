

<template >

  <div class="box">
    <h4 class="burgerName">{{burger.name}}</h4>

    <img v-bind:src="burger.imgURL" class="burgerImage" ><br>

    <br>
    <ul>
      <li style="text-align: left">{{burger.kcal+"kcal"}}</li>
      <li style="text-align: left" v-if="burger.lactose===true" class="lactose">Contains lactose</li>
      <li style="text-align: left" v-if="burger.gluten===true" class="gluten">Contains gluten</li>
    </ul>
    <div class="addRemoveButton">
      <button class="burgerButton" v-on:click="selectThisBurger(0)">
        -
      </button>
      <button class="burgerButton" v-on:click="selectThisBurger(1)">
        +
      </button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }

  },
  methods: {
    selectThisBurger: function (int) {
      if(int===1){
        console.log("ordered " + this.burger.name)
        this.amountOrdered +=1;
        console.log(this.amountOrdered)
        this.$emit("orderedBurgers", {name: this.burger.name,amount: this.amountOrdered});
      }else if(this.amountOrdered>0){
        console.log("removed " + this.burger.name)
        this.amountOrdered -=1;
        console.log(this.amountOrdered)
        this.$emit("orderedBurgers", {name: this.burger.name,amount: this.amountOrdered})
      }
    },


  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->



<style scoped>
.box{
  background-color: #374785;
  color: #ffffff;
  margin: 10px;
  padding: 20px;
  height: 400px;
  text-align: center;
  border-radius: 50px;
  text-shadow: 0 0 15px black;
}

.burgerImage{
  width: 80%;
}

.burgerName{
  font-size: 120%;
  text-decoration-line: underline;
}

.burgerButton{
  outline: 0;
  cursor: pointer;
  border: 2px solid #000;
  border-radius: 3px;
  color: #fff;
  background: #000;
  font-size: 13px;
  font-weight: 600;
  line-height: 15px;
  padding: 6px 10px;
  transition-duration: .15s;
  transition-property: all;
}

.addRemoveButton{
  display: inline-block;
  width: 80px;


}


button:hover{
  color: black;
  background: #f76c6c;
}
.gluten{
  color: #FFB5C2;
}

.lactose{
  color: #FFE156;
}
</style>
  