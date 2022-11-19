<template>
  <div>
    <section>
      <div class="image" >
        <img src="@/assets/Vector-Pattern-PNG-Image.png"  alt="Burger restaurant"
             style="width: 100%;filter: blur(2px)">
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
          <form>
            <label>Full name</label><br>
            <input class="formStyle" v-model="customerInfo.customerInput.fullName" type="text"
                   placeholder="First and Last name" required/>
            <br>
            <br>
            <label>Choose delivery location by clicking the map</label>
            <div class="mapDiv" >
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
            <input class="formStyle" v-model="customerInfo.customerInput.email" type="email"
                   required placeholder="Email"/>

            <input class="formStyle" v-model="customerInfo.customerInput.telephoneNr" type="number"
                   required placeholder="Tel. Nr."/>


            <p>
              <label>Payment method</label><br>
              <select id="payment" v-model="customerInfo.customerInput.paymentMethod" required>
                <option>Swish</option>
                <option>PayPal</option>
                <option>Debit Card</option>
              </select>
            </p>
            <br>
            <p>
              <label>Choose gender</label><br>
              <input type="radio" v-model="customerInfo.customerInput.gender"
                     value="Male" required>Male

              <input type="radio" v-model="customerInfo.customerInput.gender"
                     value="Female" required>Female

              <input type="radio" v-model="customerInfo.customerInput.gender"
                     value="Other" required>Other

              <input type="radio" v-model="customerInfo.customerInput.gender"
                     value="Do not wish to provide" required>Do not wish to provide

            </p>
            <br>

            <div>

              <button type="submit" class="orderButton" style="margin: 20px"
                      v-on:click="submitted();printCustomerInfo()" ><img src="@/assets/Shopping.png" style="left: 0"
              > ORDER</button>
              <span v-if="this.isHidden===true">Order submitted!</span>

            </div>
          </form>

        </div>
        <div class="cart">
          <h2>Cart</h2>
          <div class="scrollCart">
            <div v-for="burgerOrder in customerInfo.selectedBurger"
                 v-bind:key="burgerOrder.name">
              <li v-if="burgerOrder.amount>0">
                {{burgerOrder.amount+"x "+burgerOrder.name}}
              </li>
            </div>
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
      isHidden: false,
      burgers: burgerArray,
      customerInfo:
          {selectedBurger: [],
            location: {x:0,
              y:0},
            customerInput: {
              fullName: "",
              email: "",
              telephoneNr: "",
              paymentMethod: "",
              gender: ""
            },
          }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    printCustomerInfo: function () {
      console.log(this.customerInfo)
      socket.emit("addOrder", {orderId: this.getOrderNumber(),
            details: {x: this.customerInfo.location.x,
                      y: this.customerInfo.location.y},
            orderItems: this.customerInfo.selectedBurger,
            customerDetails: this.customerInfo.customerInput
          }
      );

    },
    submitted: function(){

        this.isHidden=true
        setTimeout(function(){
          window.location.reload();
        }, 2000);
        return this.isHidden;
    },

    setSelectedBurger: function (event) {
      for(let i=0;i<this.customerInfo.selectedBurger.length; i++){
        if(this.customerInfo.selectedBurger[i].name===event.name){
          this.customerInfo.selectedBurger[i].amount=event.amount;
          return;
        }
      }
      this.customerInfo.selectedBurger.push(event)
    },
    setLocation: function(event){
      let offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};

      this.customerInfo.location.x=event.clientX - 10 - offset.x;
      this.customerInfo.location.y=event.clientY - 10 - offset.y;
    }
  }
}
</script>

<style>
.mapDiv{
  width: 50vh;
  height: 50vh;
  overflow: scroll;
}

.map {
  width: 1920px;
  height: 1078px;
  background-image: url("@/assets/polacks.jpg");
}


body{
  width: 100%; height: 100%; margin: 0 auto; text-align: center;
  font-family: "Trebuchet MS", sans-serif;
  /*font-family: Helvetica,serif;*/
  background-color: #a8d0e6;
}

.wrapper{
  width: 100%;
  background-color: #f76c6c;
  display:inline-grid;
  grid-template-columns: 25% 25% 25%;
  justify-content: center;
  border-bottom: 5px solid black;
  border-top: 5px solid black;

}

.deliveryCart{
  background-color: #24305e;
  color: white;
  border-style: solid;
  border-color: black;
  border-radius: 50px;
  border-width: 10px;
  margin: 10px;
  padding: 30px;
  width: 60%;
  display: inline-grid;
  grid-template-columns: 60% 40%;
  /*Text-border från stack overflow*/
  /*text-shadow: 1px 0 #000, -1px 0 #000, 0 1px #000, 0 -1px #000,*/
  /*1px 1px #000, -1px -1px #000, 1px -1px #000, -1px 1px #000;*/

  text-shadow: 0 0 3px black;
}

.formStyle{
  padding: 7px;
  margin-top: 5px;
  margin-bottom: 5px;
}

.deliveryCart{
}
.delivery{
  text-align: left;
  width: 600px;
  display: inline-block;

}
.cart{
  text-align: left;
  background-color: #f76c6c;
  margin-top: 20%;
  display: inline-block;
  height: 300px;
  width: 70%;
  border-style: solid;
  border-width: 10px;
  border-color: black;
  border-radius: 50px;

  overflow: auto;
  font-size: 120%;
}

.cart li{
  padding-left: 5%;
}

.cart h2{
  text-align: center;
}


.scrollCart{

  overflow: auto;
}

.image{
  height: 200px;
  overflow: hidden;
  color: white;
  text-align: center;

}

.img-text{
  background-color: #24305e;
  position: absolute;
  top: 100px;
  width: 100%;
  margin: 0 auto;
  font-size: 300%;
  text-shadow: 0 0 20px black;
  font-family: "Trebuchet MS", sans-serif;
  letter-spacing: -2px;
  text-transform: uppercase;
}

.img-text{
  font-family: "Trebuchet MS", sans-serif;
  letter-spacing: -2px;
  border-bottom: 2px solid black;
  text-transform: uppercase;
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

.orderButton img{
  filter: invert(1);
  width: 15px;
}
button:hover{
  color: black;
  background: #f76c6c;

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

