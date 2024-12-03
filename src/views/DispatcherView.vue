<template>
    <div id="orders">
      <div id="orderList">
        <div v-for="(order, index) in orders" v-bind:key="'order'+index" class="order">
          <h3>#{{ index }}:</h3>
         <!-- #{{ key }}: {{ order.orderItems.join(", ") }}-->
          <p class="order-items">
            {{ Object.entries(order.orderItems) 
            .map(([burger, amount]) => `${burger}: ${amount} st`)
          .join(", ")}},
          </p>
          <p class="custumer-info">
            {{ order.details.name }},
            {{ order.details.email }},
            {{ order.details.paymentMethod }},
            {{ order.details.gender }}
          </p>

        </div>
        <button id="clearQueue" v-on:click="clearQueue">Clear Queue</button>
      </div>

      <div id="dots">
          <div v-for="(order, index) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" 
          v-bind:key="'dots' + index" class="dots-container">
            <!--{{ key }}-->
             <div class="dot">T</div>
             <div class="order-id">{{ "#" + order.orderId}}</div>
          </div>
      </div>
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io("localhost:3000");
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: [],
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
      this.ReciveOrders(data))
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      },
      changeStatus: function(orderId) {
        socket.emit('changeStatus', {orderId: orderId, status: "Annan status"});

      },
      ReciveOrders: function(data) {
        this.orders = data.orders;
        console.log(this.orders)
      }
    }
  }
  </script>
  <style>
  #orderList {
    top:1em;
    left:1em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(255,255,255, 0.5);
    padding: 1em;
  }

  .order {
    margin: 1px;
    padding: 0.5em;
  }
  .order-items {
    margin: 1px;
  }
  .costumer-info {
    margin: 1px;
  }

  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
    background-image: url('/img/polacks.jpg');
  }
  .dots-container {
    position: absolute;
    transform: translate(-50%, -50%);
    
  }

  .dot{
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width: 20px;
    text-align: center;
    z-index: 10;
    top: -10px;
    left: -7px;
  }

  .order-id {
    position: absolute;
    transform: translateX(-50%);
    z-index: 5;
    top: 7px;
    left: 2px;
    border: double deeppink;
    border-radius: 5px;
    color: deeppink;
    background: lavender;
    
  }

   #clearQueue:hover {
    background-color: palevioletred;
    color: azure;
    cursor: pointer;
}
  /*
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
    */
  </style>
  