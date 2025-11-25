<template>
    <div id="orders">
      <div id="orderList">
     
        
      
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots">
          <div class="target" v-bind:style="{ left: deliveryPos.x + 'px', top: deliveryPos.y + 'px'}">
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
        deliveryPos: { x: null, y: null },
        orders: {}
      }
    },
    created: function () {
      socket.on('addOrder', data => {
      console.log("Dispatcher received:", order.deliveryPos);
      this.deliveryPos = order.deliveryPos;
      });
    },
    methods: {
      clearQueue: function () {
        this.orders = {};
        socket.emit('clearQueue');
        this.deliveryPos = { x: 0, y: 0 };
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
  </style>
  