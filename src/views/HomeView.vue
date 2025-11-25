<template>
<div>
  <header>
    <h1>Welcome to Bomblaclat Burger</h1>
  </header>
  <br>
  <hr>
  <br>
  <main>
    <section id="burgers">
    <h2>Select bombaburger:</h2>
    <nav>Menu items</nav>
    <div class="wrapper">
    <Burger
        v-for="b in burgers"
        :key="b.name"
        :burger="b"
        @update-amount="updateAmount"
      />
    </div>
    </section>
  </main>
<br>
<hr>
  <main>
    <section id="contact">
    <h3>Costumah information</h3>
    <p>This is were you provide the information required for your delivery.</p>
        <form>
<p>
    <label for="firstname">Full Name</label><br>
    <input type="text" id="fullname" v-model="fn" required="required" placeholder="Full name">
</p>
<p>
    <label for="email">E-mail Address</label><br>
    <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
</p>
<p> 
   <label for="paymentmethod">Payment Method</label> <br>
   <select v-model="rcp">
       <option>Klarna</option>
       <option>Natura</option>
       <option>Apple Pay</option>
       <option>Me no Pay Me Rich Millionaire</option>
   </select>
</p>
<div style="display: inline-block">
    <p>Provide your Gender:</p>
    <input type="radio" id="female" v-model="gender" value="Female" checked />
    <label for="female">Female</label>

    <input type="radio" id="male" v-model="gender" value="Male">
    <label for="male">Male</label>

    <input type="radio" id="notsay" v-model="gender" value="Do not wish to provide">
    <label for="notsay">Do not wish to provide</label>
  </div>
        </form>
    
  <br>
  <div id="map">
    <div class="map-inner" @click="setDeliveryPos">
    <div class="dot" 
         :style="{ left: deliveryPos.x + 'px', top: deliveryPos.y + 'px' }">
    T</div>
  </div>
  </div>
    <button type="button" class="send-order-button" v-on:click="addOrder"> 
    Send Order 
    <img src="https://img.favpng.com/8/2/5/shaka-sign-sign-language-hawaiian-language-hang-ten-png-favpng-QbjqvbsMAjbWwkNkLWmSzkkfk.jpg" style="width: 70px">
    </button>
  </section>
  </main>
<br>
<hr>

  <footer>&copy;2025 Bomblaclat Burger. All rights reserved.
    </footer>
</div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'


const socket = io("localhost:3000");

class MenuItem {
  constructor(name, url,price, ingredients) {
    this.name = name;
    this.url = url;
    this.price = price;
    this.ingredients = ingredients
  }
}

const BurgerArray = [
  new MenuItem("TheRastaclatFavorite","https://www.foodandwine.com/thmb/DI29Houjc_ccAtFKly0BbVsusHc=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/crispy-comte-cheesburgers-FT-RECIPE0921-6166c6552b7148e8a8561f7765ddf20b.jpg","$1000 dollar",[{ name: "Beef", allergen: true },
      { name: "Avocado", allergen: false },
      { name: "Red Onion", allergen: false },
      { name: "Egg", allergen: true },
      { name: "Brioche Bun", allergen: false }]),
  new MenuItem("Originalbombaclat","https://www.certifiedangusbeef.com/_next/image?url=https%3A%2F%2Fappetizing-cactus-7139e93734.media.strapiapp.com%2FSweet_Jamaican_Burger_6bc6c7e649.jpeg&w=1920&q=75","$million dollar",[{ name: "Beef", allergen: true },
    { name: "Corn", allergen: false },
    { name: "Lettuce", allergen: false },
    { name: "Mayonnaise", allergen: true },
    { name: "Sesame Bun", allergen: true }]),
  new MenuItem("TheRichMillionaire","https://www.honestburgers.co.uk/wp-content/uploads/2021/01/honest-beef-bacon-red-leicester-cheese-apricot-and-scoth-bonnet-ketchup-plantain-and-curry-mayo-page.jpg","$1 000 000 dollar",[{ name: "Beef", allergen: true },
    { name: "Corn", allergen: false },
    { name: "Lettuce", allergen: false },
    { name: "Mayonnaise", allergen: true },
    { name: "Sesame Bun", allergen: true }])
];


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data() {
    return {
      burgers: menu,
      fn: "",
      em: "",
      rcp: "",
      gender: "",
      amountOrdered: {},
      deliveryPos: { x: null, y: null}
    };  
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    setDeliveryPos (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.deliveryPos = { x: event.clientX - 10 - offset.x,
                           y: event.clientY - 10 - offset.y };
                           console.log("Delivery position set!",this.deliveryPos)
    },

updateAmount (payloadInfo) {
  this.amountOrdered[payloadInfo.name] = payloadInfo.amount; 
    },

addOrder() {
  socket.emit('addOrder', {
    orderId: this.getOrderNumber(),
    orderItems: this.amountOrdered,
    fn: this.fn,
    em: this.em,
    rcp: this.rcp,
    gender:this.gender,
    deliveryPos:this.deliveryPos,
    }
  );
    
  console.log("Full name:", this.fn);
  console.log("Email:", this.em);
  console.log("Payment method:", this.rcp);
  console.log("Gender:", this.gender);
  console.log("Amount ordered:", this.amountOrdered);
  console.log("Delivery pos:", this.deliveryPos);
}  
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Luckiest+Guy&family=Bangers&family=Chewy&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Rock+Salt&display=swap');

body {
    font-size: 15pt;
    font-family: 'Cormorant', serif;
    background-attachment:fixed;
    background-image: url("https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTl0ZUOsV0lZVPukaYt87EvOZi8XFvUiEuVQQ&s");
}

p {
    color: red;
    margin: 20px;
}
.allergen {
   color: #ff5500;
   text-transform: uppercase;
}

#burgers {
    background-color: black;
    color: white;
    border: 2px solid #f6de07;
}


#burgers div {
  margin: 20px;
  padding: 10px;
}

#contact {
    background-color: white;
    border: 2px solid #f6de07;
}

section * h3 {
    font-size: 19pt;
    font-family: 'Rock Salt';
    margin: 20px;
}

h2 {
    font-size: 15pt;
    font-family: 'Rock Salt';
    margin: 7px;
}
.send-order-button:hover {
    background-color: rgb(105, 223, 105);
    color: black;
    font-weight: bold;
    font-family: 'Rock Salt';
    cursor: pointer;
    transform: scale(1.8);
}


section {
  margin: 30px;

  padding: 20px;
}

button {
  margin-top: 20px;
}

main, header, footer, nav ul {
    max-width: 75rem;
    margin: 0 auto 0 auto;
}

main {
    background-color: rgb(11, 106, 68);
}

/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */

header {
    background-image: url("https://jamaicans.com/wp-content/uploads/Reasons-Jamaican-Are-The-Most-Chill-People-In-The-World.jpeg");
    background-size: cover;
    overflow: hidden;
    width: 100%;
    height: 370px;
    padding-top: 50px;
}

header h1 {
    width:40rem;
    margin: 0 auto;
    text-align: center;
    font-family: 'Rock Salt';
    font-size: 30pt;
    color: rgb(17, 96, 17); 
}
.wrapper {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 1em;
    padding: 0;
}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: grey;
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

#map {
  width: 500px; 
  height: 300px;
  border: 2px solid black;
  margin-top: 20px;
  overflow: scroll;
}
.map-inner {
  position: relative;   
  width: 1920px;        
  height: 1080px;
  background: url("/img/polacks.jpg");
}
 .dot {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
  }
</style>