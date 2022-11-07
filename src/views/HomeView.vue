<template>
  <div>
    <section>
      <div class="image" >
        <img src="https://static.dezeen.com/uploads/2020/01/yuzu-burger-restaurant-haf-studio-reykjavik-iceland_dezeen_2364_col_12.jpg" alt="Burger restaurant"
             style="opacity: 70%">
        <h1 class="img-text">Welcome to Burger Kitchen</h1>
      </div>
    </section>
    <main>

        <div class="wrapper" >
          <div class="box a">
            <h4>Cheeseburger</h4>
            <img src="https://s7d1.scene7.com/is/image/mcdonalds/t-mcdonalds-McDouble-1:product-header-desktop?wid=829&hei=455&dpr=off"
                 alt="Cheeseburger" style="width: 200px;height: 130px">
            <ul style="text-align: left">
              <li>600 kcal</li>
              <li>Cheezy</li>
              <li class="lactose">Contains lactose</li>
              <li class="gluten">Contains gluten</li>
            </ul>
          </div>

          <div class="box b">
            <h4>Bacon-burger</h4>
            <img src="https://s7d1.scene7.com/is/image/mcdonalds/t-mcdonalds-qpc-bacon:product-header-desktop?wid=829&hei=455&dpr=off"
                 alt="Cheeseburger" style="width: 200px;height: 130px">
            <ul style="text-align: left">
              <li>750 kcal</li>
              <li>For the enthusiast</li>
              <li class="lactose">Contains lactose</li>
              <li class="gluten">Contains gluten</li>
            </ul>
          </div>

          <div class="box c">
            <h4>Plain Burger</h4>
            <img src="https://s7d1.scene7.com/is/image/mcdonalds/t-mcdonalds-Hamburger:product-header-desktop?wid=829&hei=455&dpr=off"
                 alt="Cheeseburger" style="width: 200px;height: 130px">
            <ul style="text-align: left">
              <li>500 kcal</li>
              <li>The classic</li>
              <li class="gluten">Contains gluten</li>
            </ul>
          </div>

        </div>





      <h2>Delivery information</h2>
      <hr>

        <div class="delivery">
          <form style="text-align: left">
            <p>
              <label>Full name</label><br>
              <label for="firstname"></label><input type="text" id="firstname" name="fn" required="required" placeholder="First and Last name">
            </p>
            <p>
              <label>Address</label><br>
              <label for="street"></label><input type="text" id="street" name="ad" required="required"
                                                 placeholder="Street name">
              <label for="city"></label><input type="text" id="city" name="ci" required="required"
                                               placeholder="City">
            </p>
            <p>
              <label for="house number"></label><input type="number" id="house number" name="hn" required="required" placeholder="Nr.">
            </p>

            <p>
              <label>Email & Telephone Nr.</label><br>
              <label for="email"></label><input type="email" id="email" name="em" required="required" placeholder="Email">
              <label for="number"></label><input type="number" id="number" name="nr" required="required" placeholder="Tel. Nr.">
            </p>
          </form>
        </div>

        <p class="delivery" style="text-align: left">
          <label for="payment">Payment method</label><br>
          <select id="payment" name="pay">
            <option>Swish</option>
            <option>Klarna payment plan</option>
            <option>KRITA</option>
          </select>
        </p>
        <br>
        <p class="delivery" style="text-align: left">
          <label>Choose gender</label><br>
          <input type="radio" name="gender" id="Male" value="Male">
          <label for="Male">Male</label>
          <input type="radio" name="gender" id="Female" value="Male">
          <label for="Female">Female</label>
          <input type="radio" name="gender" id="Other" value="Male">
          <label for="Other">Other</label>
          <input type="radio" name="gender" id="Do not wish to provide" value="Male">
          <label for="Do not wish to provide">Do not wish to provide</label>
        </p>
        <br>

        <button type="submit" class="button" style="margin: 20px">ORDER</button>
    </main>
  </div>

</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io();


// eslint-disable-next-line no-unused-vars
function MenuItem(productName,imgURL,kcal,gluten,lactose){
  this.productName=productName;
  this.imgURL=imgURL;
  this.kcal=kcal;
  this.gluten=gluten;
  this.lactose=lactose;
}


let cheeseBurger= new MenuItem("Cheeseburger"
                    ,"https://s7d1.scene7.com/is/image/mcdonalds/t-mcdonalds-McDouble-1:product-header-desktop?wid=829&hei=455&dpr=off"
                    ,600,true,true);
let baconBurger = new MenuItem("Bacon burger"
    ,"https://s7d1.scene7.com/is/image/mcdonalds/t-mcdonalds-qpc-bacon:product-header-desktop?wid=829&hei=455&dpr=off"
    ,600,false,true);



const burgerArray = [{name: "Cheeseburger",cheeseBurger},{name: "Bacon burger",baconBurger}]

console.log(burgerArray)


export default {
  name: 'HomeView',
  components: {
    // eslint-disable-next-line vue/no-unused-components
    Burger
  },
  data: function () {
    return {
        burgers: burgerArray
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
    }
  }
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }


  body{
    font-family: Helvetica;


  }


  /*Grid Layout*/

  .wrapper{
    display:grid;
    grid-gap: 10px;
    grid-template-columns: 300px 300px 300px;
    margin: auto;
  }

  .box{
    background-color: #444;
    color: #ffffff;
    border-radius: 5px;
    padding: 20px;
    height: 300px;
    text-align: center;
  }
  .a{
    grid-column: 1;
  }
  .b{
    grid-column: 2;
  }
  .c{
    grid-column: 3;
  }






  section{

  }

  .gluten{
    color: indianred;
  }

  .lactose{
    color: cadetblue;
  }

  .image{
    height: 400px;
    width: 100%;
    overflow: hidden;
    color: white;
  }

  .img-text{
    position: absolute;
    top: 200px;
    width: 100%;
    margin: 0 auto;
    font-size: 300%;
    text-shadow: 0 0 8px black;
  }

  .burgers{
    background-color: black;
    color: white;
    width: 100%;
    margin: 0 auto;
  }


  .delivery{

  }

  button {

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
    transition-timing-function: cubic-bezier(.4, 0, .2, 1);
  }
  button:hover{
    color: #000;
    background: rgb(255, 218, 87);
  }

</style>

