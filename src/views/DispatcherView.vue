<template>
    <div id="orders">
      <div id="orderList">
        <div
        v-for="(order, orderId) in orders"
        :key="'order' + orderId">
        #{{ orderId }} <br>
        Items:
              <ul>
                <li
                  v-for="(amount, burgerName) in order.orderItems"
                  :key="burgerName">
                  {{ amount }} × {{ burgerName }}
                </li>
              </ul> 
        <p class="personalInfo"> Name: {{ order.fn }} </p>
        <p class="personalInfo">Email: {{ order.em }}</p>
        <p class="personalInfo">Payment Method: {{ order.rcp }}</p>
        <p class="personalInfo">Gender: {{ order.gender }}</p>
        <hr></hr>
      </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots">
          <div
        v-for="(order, orderId) in orders"
        :key="'dots' + orderId"
        class="target"
        :style="{ left: order.deliveryPos.x + 'px', top: order.deliveryPos.y + 'px' }"
      >
            T
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
        orders: {}
      }
    },
    created: function () {
      socket.on('currentQueue', data => {
      console.log("Dispatcher received:", data);
      this.orders = data.orders;
      });
    },
    methods: {
      clearQueue: function () {
        this.orderItems = {};
        socket.emit('clearQueue');
        this.orders = { };
      },
      changeStatus: function(orderId) {
        socket.emit('changeStatus', {orderId: orderId, status: "Annan status"});

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
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  
  .personalInfo {
    color: grey;
    font-size: 15px;
    margin: 2px 0;
  }
  </style>
  