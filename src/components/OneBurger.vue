

<template >

  <div class="box">
    <h4>{{burger.name}}</h4>

    <img v-bind:src="burger.imgURL" style="width: 200px;height: 130px"><br>

    <br>
    <ul>
      <li style="text-align: left">{{burger.kcal+"kcal"}}</li>
      <li style="text-align: left" v-if="burger.lactose===true" class="lactose">Contains lactose</li>
      <li style="text-align: left" v-if="burger.gluten===true" class="gluten">Contains gluten</li>
    </ul>
    <div class="addRemoveButton">
      <button class="burgerButton" v-on:click="selectThisBurger()">
        Add to cart
      </button>

<!--      <button class="burgerButton" v-if="amountOrdered>0" v-on:click="removeThisBurger()">-->
<!--        Remove-->
<!--      </button>-->
<!--      <p>{{amountOrdered}}</p>-->
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
    // addBurger: function () {
    //   console.log("ordered "+ this.burger.name)
    //   this.amountOrdered += 1;
    //   console.log(this.amountOrdered);
    //   this.$emit("orderedBurgers", {
    //         name: this.burger.name,
    //         amount: this.amountOrdered
    //       }
    //   );
    // },
    selectThisBurger: function () {
      console.log("ordered " + this.burger.name)
      this.amountOrdered +=1;
      this.$emit("orderedBurgers", {name: this.burger.name,amount: this.amountOrdered});
    },

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->



<style scoped>
.box{
  background-color: #41278F;
  color: #ffffff;
  margin: 10px;
  padding: 20px;
  height: 400px;
  text-align: center;
  border-radius: 25px;
  text-shadow: 0 0 15px black
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
  color: white;
  background: #41278F;
}
.gluten{
  color: #FFB5C2;
}

.lactose{
  color: #FFE156;
}
</style>
  