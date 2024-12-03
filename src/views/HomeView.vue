<template>

<div>
<header>
     <h1 style="text-align: center;">Välkommen till Bruger Queen!</h1>
</header>
  <br />
    <main>
        <section>
            <h2>Välj Hamburgare:</h2>
                <p>Här genomför du ditt val av burgare.</p>
                <div class="burger-container">
                      <Burger 
                        v-for="(burger, index) in menu"
                        :key="burger.name"
                        :burger="burger" 
                        v-on:orderedBurger="addToOrder"/>
                </div>  
        </section>
        <br>

        <section id="kundinfo">
            <h3>Kundinformation</h3>
                <p>Här fyller du i den nödvändigainformationen.</p>
                <br>
                <p><strong>Leveransinformation:</strong></p>
                <br>
                <p>
                    <label for="name">För- och efternamn:</label><br>
                    <input type="text" id="name" name="fn" 
                    v-model="fromData.name" required="required" placeholder="För- och efternamn">
                </p>
                <p>
                    <label for="email">E-mail address</label><br>
                    <input type="email" id="email" name="em" 
                    v-model="fromData.email" required="required" placeholder="E-mail address">
                </p>
                
        </section>     
                <p>
                 
                    <label for="payment">Betalningsmetod:</label><br>
                    <select id="payment" name="pay" v-model="fromData.paymentMethod">
                        <option>Swish</option>
                        <option>Wire tranfer</option>
                        <option>Paypal</option>
                        <option>Klarna faktura</option>
                    </select>
                 </p>

                 <p>Vänligen välj ditt kön:</p>
                 <label for="gender">Kön:</label><br>
                 <select id="gender" v-model="fromData.gender">
                  <option>Kvinna</option>
                  <option>Man</option>
                  <option>Icke-binär</option>
                  <option>Icke avslöja</option>
                </select>

      <!--Här kartgrjer-->
      <p>
        Klicka vart på kartan du vill beställa till och tryck sedan "Place order" för att skicka beställningen.
      </p>

      <div id="map-container">
        <div id="map" v-on:click="setLocation">
        <!-- Visa en "T" på platsen användaren klickade -->
        <div
        v-if="location.x !== 0 && location.y !== 0"
            class="target"
            v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">
            T
        </div>
        </div>
      </div>

        <br>
        <button id="submit-button" type="button" v-on:click="submitOrder">
            <img src="data:image/png;base64,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"
            width="30px"/>
            Place order
          </button>
    </main>
    <hr>
    <footer><small><strong>&copy;2024 Burger Queen CO</strong></small></footer>
  </div>
</template>

<script>
import menu from "/Users/hannawerre/Desktop/1MD031-lab-2024/src/assets/menu.json";
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'


function MenuItem(name, imageUrl, kCal, lactose, gluten) {
  this.name = name;
  this.imageUrl = imageUrl;
  this.kCal = kCal;
  this.description = description;
  this.onBurger = onBurger;
  this.sides = sides;
  this.lactose = lactose;
  this.gluten = gluten;
  this.vegetarisk = vegetarisk;
}


const socket = io("localhost:3000");


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      menu,
      orderedBurgers: {},
      location: {x:0, y:0},
      fromData: {name: "",
        email: "",
        paymentMethod: "Swish",
        gender: "Icke avslöja"}
    }
  },
  methods: {
    addToOrder(event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation(event) {
      const rect = event.target.getBoundingClientRect();
      this.location.x = event.clientX - rect.left;
      this.location.y = event.clientY - rect.top;
    },
    resetFields() {
      // Återställ fält, lyckas tyvärr inte återställa countern på antalet burgare
      this.fromData = {
        name: "",
        email: "",
        paymentMethod: "Swish",
        gender: "Icke avslöja"
      };
      this.location = { x: 0, y: 0 };
    },
    submitOrder() {
    
    const order = {
      orderId: this.getOrderNumber(), // random ID
      details: {
        x: this.location.x,
        y: this.location.y,
          name: this.fromData.name,
          email: this.fromData.email,
          paymentMethod: this.fromData.paymentMethod,
          gender: this.fromData.gender,
      },
      
        orderItems: this.orderedBurgers,
    }
    console.log(order)
    console.log(order.details.x, order.details.y);
    socket.emit("addOrder",  order );
    this.resetFields();
  },
  getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    
  }
};
    /*
    addOrder: function (event) {
      
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      
    }
      socket.emit("addOrder", order);
      console.log("Skickad order:", order);
  }
      */
  


</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
  @import url('https://fonts.googleapis.com/css2?family=Edu+AU+VIC+WA+NT+Pre:wght@400..700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');
body {
    font-family: 'Roboto', sans-serif; /* Default font for the entire body */
    font-size: 12pt;
    background-image: url("https://media.istockphoto.com/id/868234810/vector/cute-cartoon-burgers-seamless-pattern-on-color-background-flat-design-vector-illustration.jpg?s=612x612&w=0&k=20&c=3M54ln1ozQy150e1gKIwNLHavnmMiIKEYfhnYoleiQs=");
}

p {
    color: DeepPink;
}

header h1 {
    font-family: 'Edu AU VIC WA NT Pre', cursive; /* Specific font for the header */
    font-weight: 400;
    font-size: 40pt;
    text-align: center;
    margin: 0 auto;
    width: 40rem;
}

main, header, footer, nav ul {
    max-width: 100rem;
    margin: 0 auto 0 auto;
}
main {
    padding: 10px;
    margin: 20px;
    border: 3px solid deeppink;
    border-radius: 10px;
    background-color: lavender;
}



header {
    background-image: url("https://img.freepik.com/premium-vector/hamburger-purple-background-with-purple-background_792213-41.jpg");
    background-size: cover;
    overflow: hidden;
    min-width:500px;
    min-height: auto;
    opacity: 0.9;
    margin: 25px 0;
    padding: 10px;
    margin-left: 20px;
    margin-right: 20px;
    border: 5px double deeppink;
}

header h1 {
    width:40rem;
    margin: 0 auto;
    text-align: center;
}


.burger-container {
    background-color: purple;
    color: black;
    display: grid; 
    grid-template-columns: 33% 33% 33%; /* Flexibla kolumner */
    grid-gap: 10px; 
    margin: 20px auto; 
    border: 5px double deeppink;
    padding: 20px;
    border-radius: 10px;
    width: calc(100% - 50px);
}


#kundinfo {
    border: 5px double deeppink;
    padding: 20px;
    border-radius: 10px;
}

#map-container {
  width: 100%;
  max-width: 1500px;
  height: 400px;
  overflow: scroll;
  border: 2px solid black;
  margin: 20px auto;
  position: relative;
}
#map {
  background: url("/img/polacks.jpg") no-repeat center center;
  background-size: contain;
  width:1920px;
  height: 1078px;
  position: relative;
  cursor: grab; 
}
.target {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 15px;
  font-size: 13px;
  text-align: center;
  transform: translate(-50%, -50%);
}
#submit-button:hover {
    background-color: palevioletred;
    color: azure;
    cursor: pointer;
}

</style>