<template>
  <div>
     
    <div class="burger-item a">
      
      <p style="font-weight: bold;">{{ burger.name }} {{ burger.kCal }} kCal</p>
      <img :src="burger.imageUrl" alt="Bild på {{ burger.name }}">
      <ul>
        <li>{{ burger.description }}</li>
        <li>{{ burger.onBurger }}</li>
        <li>Tillbehör: {{ burger.sides }}</li>
        <br>
          <section class="allergier">
            <li><span id="allergi">Allergener:</span></li>
            <ul>
              <li v-if="burger.lactose">Laktos</li>
              <li v-if="burger.gluten">Gluten</li>
              <li v-if="burger.vegetarisk">Vegetarisk</li>
            </ul>
          </section>
      </ul>
    </div>

    <div class="order-controls">
    <button @click="decreaseOrder">-</button>
    <span>{{ amountOrdered }}</span>
    <button @click="increaseOrder">+</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: {
      type: Object,
    required: true,
    }
  },
  data() {
    return{
      amountOrdered: null,
    }
  },


  methods: {
    increaseOrder() {
      this.amountOrdered++,
      this.$emit("orderedBurger", {
        name: this.burger.name,
        amount: this.amountOrdered,
      });
    },
    decreaseOrder() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit("orderedBurger", {
          name: this.burger.name,
          amount: this.amountOrdered,
        });
      }
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


.burger-item {
    text-align: center; 
    margin: 10px auto;
    padding: 10px;
    background-color: lavender;
    border: 2px solid deeppink;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    width: 300px;
    
}


.burger-item ul {
    text-align: left; 
    margin: 10px; 
    padding: 20px; 
    list-style-type: none;
}
.allergier ul {
  margin: 0;
  padding: 0;
}

.burger-item img {
    width: 100%; 
    max-width: 400px;
    height: auto;
    display: block;
    margin: 10px auto;
    border-radius: 8px;
}

.allergier {
    color: red
}
#allergi {
    text-transform: uppercase;
}

.order-controls {
  display: flex;
  color: white; 
  align-items: center;
  margin: 10px;
  padding: 10px;
}
</style>