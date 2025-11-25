<template>
  <div class="OneBurger">
        <h3>{{burger.name}}</h3>
        <img :src="burger.url" style="width: 200px" >
        <ul>
  <li 
    v-for="item in burger.ingredients"
    :key="item.name"
  >
    <span :class="{ allergen: item.allergen }">
      {{ item.name }}
    </span>
  </li>
</ul>
        <div class="price">{{burger.price}}</div>
        <p>Amount: {{ amountOrdered }}</p>
        <button class="remove-amount-button" @click="decreaseAmount">Remove</button>
        <button class="add-amount-button" @click="increaseAmount">Add</button>
        </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
data : function () {
  return {
    amountOrdered: 0
  };
},
  methods: {
    increaseAmount() {
      this.amountOrdered++;
      this.$emit('update-amount', {name:this.burger.name, amount:this.amountOrdered});
    },
    decreaseAmount() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit('update-amount', {name:this.burger.name, amount:this.amountOrdered});
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.price {
    color: red;
    margin: 20px;
}
.allergen {
   color: #ff5500;
   text-transform: uppercase;
}
.remove-amount-button, .add-amount-button {
    margin-top: 10px;
    margin-right: 10px;
    background-color: #f6de07;
    cursor: pointer;
}

.OneBurger h3 {
  font-size: 14pt;
  font-family: 'Rock Salt';
  margin: 20px;
}
.OneBurger {
  background-color: black;
  color: white;
  border: 2px solid #f6de07;
  margin: 20px;
  padding: 10px;
  text-align: left;
}

.OneBurger img {
  margin-left: 20px;
}
</style>