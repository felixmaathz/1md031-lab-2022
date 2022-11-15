<template>
  <div>
    <section>
      <div class="image" >
        <img src="https://cdn.dribbble.com/users/3558835/screenshots/9197759/seamless_background_pattern-01.jpg" alt="Burger restaurant"
             style="opacity: 70%">
        <h1 class="img-text">Welcome to Burger Kitchen</h1>
      </div>
    </section>
    <main>

      <div class="wrapper">
        <Burger ref="burgerComponent"
                v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurgers="setSelectedBurger($event)"
        />
      </div>


      <div class="deliveryCart">
        <div class="delivery">
          <h2>Delivery information</h2>
          <label>Full name</label><br>
          <input type="text" id="name" placeholder="First and Last name"/>
          <br>
          <br>
          <label>Choose delivery location by clicking the map</label>
          <div class="mapDiv">
            <div
                class="map" id="dots" v-on:click="setLocation">
              <div v-bind:style="{left: customerInfo.location.x + 'px',
                                  top: customerInfo.location.y + 'px'}">
                T
              </div>
            </div>
          </div>

          <!--          <input type="text" id="street" required="required" placeholder="Street name"/>-->
          <!--          <input type="text" id="city" required="required" placeholder="City"/>-->
          <br>
          <label>Email & Telephone Nr.</label><br>
          <input type="email" id="email" required="required" placeholder="Email"/>

          <input type="number" id="number"  required="required" placeholder="Tel. Nr."/>


          <p>
            <label>Payment method</label><br>
            <select id="payment" v-model="pay">
              <option>Swish</option>
              <option>Klarna payment plan</option>
              <option>KRITA</option>
            </select>
          </p>
          <br>
          <p>
            <label>Choose gender</label><br>
            <input type="radio" name="gender" id="Male">
            <label for="Male">Male</label>
            <input type="radio" name="gender" id="Female" >
            <label for="Female">Female</label>
            <input type="radio" name="gender" id="Other">
            <label for="Other">Other</label>
            <input type="radio" name="gender" id="Do not wish to provide">
            <label for="Do not wish to provide">Do not wish to provide</label>
          </p>
          <br>
          <div>
            <button type="submit" class="orderButton" style="margin: 20px"
                    v-on:click="printCustomerInfo()" ><img src="src/assets/Shopping.png">ORDER</button></div>
        </div>
        <div class="cart">
          <h2 style="position: absolute">Cart</h2>
          <div class="scrollCart">
            <li v-for="burgerOrder in customerInfo.selectedBurger" v-bind:key="burgerOrder.name">
              <button v-on:click="removeBurger(burgerOrder)" ref="removeButton">X</button>
              {{burgerOrder}}
            </li>
          </div>

        </div>



      </div>
    </main>
  </div>

</template>

<script>

import io from 'socket.io-client'
import Burger from "@/components/OneBurger";
import menu from "@/assets/menu.json";

const myData = menu;

const socket = io();

// eslint-disable-next-line no-unused-vars
// function MenuItem(productName,imgURL,kcal,gluten,lactose){
//   this.productName=productName;
//   this.imgURL=imgURL;
//   this.kcal=kcal;
//   this.gluten=gluten;
//   this.lactose=lactose;
// }

const burgerArray = [myData[0], myData[1], myData[2]]
export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgerArray,
      customerInfo:
          {selectedBurger: [{
              name: "",
              amount: 0
              }],
            fullName: "",
            email: "",
            telephoneNr: "",
            paymentMethod: "",
            location: {x:0,
                       y:0}}
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
            details: { x: event.clientX - 10 - offset.x,
              y: event.clientY - 10 - offset.y },
            orderItems: ["Beans", "Curry"]
          }
      );
    },
    printCustomerInfo: function () {


      this.customerInfo.fullName=document.getElementById("name").value;
      this.customerInfo.email=document.getElementById("email").value;
      this.customerInfo.telephoneNr=document.getElementById("number").value;
      this.customerInfo.paymentMethod=document.getElementById("payment").value;

      console.log(this.customerInfo)

      socket.emit("addOrder", { orderId: this.getOrderNumber(),
            details: {  x: this.customerInfo.location.x,
                        y: this.customerInfo.location.y},
            orderItems: ["Beans", "Curry"]
          }
      );

    },

    setSelectedBurger: function (event) {
      console.log(event.name)
      console.log(event.amountOrdered)
      console.log(this.customerInfo.selectedBurger)
      this.customerInfo.selectedBurger.push(event)



      // console.log(event.amount)
      // if(this.customerInfo.selectedBurger.includes(event.name)){
      //   const index = this.customerInfo.selectedBurger.findIndex(event.name);
      //   this.customerInfo.selectedBurger[index].amount=event.amount;
      //
      // }else {
      //   this.customerInfo.selectedBurger.push(event)
      // }
      // console.log(this.customerInfo.selectedBurger)
    },

    countBurgers: function(burger){
      let burgerCount=0;
      for(let i=0;i<this.selectedBurger.length;i++){
        if(burger===this.selectedBurger[i]){
          burgerCount+=1;
        }
      }
      return burgerCount;
    },
    removeBurger: function(burger){
      const index = this.customerInfo.selectedBurger.indexOf(burger)
      this.customerInfo.selectedBurger.splice(index,1)



      // for(let i=0;i<this.customerInfo.selectedBurger.length;i++){
      //   if(burger===this.customerInfo.selectedBurger[i]){
      //
      //   }
      // }

    },
    setLocation: function(event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};

      this.customerInfo.location.x=event.clientX - 10 - offset.x;
      this.customerInfo.location.y=event.clientY - 10 - offset.y;
    }

  }
}
</script>

<style>
.mapDiv{
  width: 400px;
  height: 400px;
  overflow: scroll;
}

.map {
  width: 1920px;
  height: 1078px;
  background-image: url("@/assets/polacks.jpg");
}


body{
  width: 100%; height: 100%; margin: 0 auto; text-align: center;
  font-family: Helvetica,serif;
  background-color: #FFB5C2;
}

.wrapper{
  width: 100%;
  background-color: #FFD8A9;
  display:inline-grid;
  grid-template-columns: 25% 25% 25%;
  justify-content: center;
}

.deliveryCart{

  border-style: solid;
  border-radius: 50px;
  margin: 10px;
  padding: 30px;
  width: 60%;
  display: inline-grid;
  grid-template-columns: 60% 40%;
}

.delivery{
  text-align: left;
  width: 600px;
  display: inline-block;

}
.cart{
  text-align: left;
  background-color: #FFD8A9;
  margin-top: 20%;
  display: inline-block;
  height: 600px;
  width: 300px;
  border-style: solid;
  border-radius: 50px;
  padding-left: 5%;
  overflow: auto;
}

.scrollCart{
  margin-top: 30%;
  height: 75%;
  overflow: auto;
}

.image{
  height: 200px;
  overflow: hidden;
  color: white;
  text-align: center;

}

.img-text{
  position: absolute;
  top: 100px;
  width: 100%;
  margin: 0 auto;
  font-size: 300%;
  text-shadow: 0 0 20px black;
}

.emptyButton{
  display: inline-block;
  outline: 0;
  cursor: pointer;
  border: 2px solid #000;
  border-radius: 3px;
  color: #fff;
  background: #000;
  font-size: 20px;
  font-weight: 600;
  line-height: 28px;
  padding: 6px 10px;
  text-align: center;
  transition-duration: .15s;
  transition-property: all;
  position: absolute;
  bottom: 20px;
}

.orderButton {
  display: inline-block;
  outline: 0;
  cursor: pointer;
  border: 2px solid #000;
  border-radius: 3px;
  color: #fff;
  background: #000;
  font-size: 20px;
  font-weight: 600;
  line-height: 28px;
  padding: 12px 20px;
  text-align: center;
  transition-duration: .15s;
  transition-property: all;
}
button:hover{
  color: white;
  background: #41278F;
}

#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}

#dots div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}

</style>

