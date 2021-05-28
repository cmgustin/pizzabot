<template>
  <v-container>
    <v-card class="wrap mt-5 mb-5">
      <div>
        <v-select
          v-model="toppingsQty"
          :items="[1, 2, 3, 4, 5, 6]"
          label="How many toppings?"
          outlined
          dense
        ></v-select>
      </div>

      <div class="mb-5">
        <label class="d-block mb-2">
          <strong class="mr-4">Meats</strong>
          <a @click="toggleAll(meatToppings, true)"><small>All</small></a> 
          <span> / </span>
          <a @click="toggleAll(meatToppings, false)"><small>None</small></a>
        </label>
      
        <span 
          class="v-chip v-chip--clickable v-chip--no-color theme--light v-size--default mr-2 mb-4"
          :class="{'v-chip--active primary--text': topping.active}"
          v-for="topping in meatToppings"
          :key="topping.name"
          @click="topping.active = !topping.active"
        >
          {{ topping.name }}
        </span>
      </div>

      <div class="mb-5">
        <label class="d-block mb-2">
          <strong class="mr-4">Veggies</strong>
          <a @click="toggleAll(veggieToppings, true)"><small>All</small></a> 
          <span> / </span>
          <a @click="toggleAll(veggieToppings, false)"><small>None</small></a>
        </label>
      
        <span 
          class="v-chip v-chip--clickable v-chip--no-color theme--light v-size--default mr-2 mb-4"
          :class="{'v-chip--active primary--text': topping.active}"
          v-for="topping in veggieToppings"
          :key="topping.name"
          @click="topping.active = !topping.active"
        >
          {{ topping.name }}
        </span>
      </div>

      <div class="mb-5">
        <label class="d-block mb-2">
          <strong class="mr-4">Cheeses</strong>
          <a @click="toggleAll(cheeseToppings, true)"><small>All</small></a> 
          <span> / </span>
          <a @click="toggleAll(cheeseToppings, false)"><small>None</small></a>
        </label>
      
        <span 
          class="v-chip v-chip--clickable v-chip--no-color theme--light v-size--default mr-2 mb-4"
          :class="{'v-chip--active primary--text': topping.active}"
          v-for="topping in cheeseToppings"
          :key="topping.name"
          @click="topping.active = !topping.active"
        >
          {{ topping.name }}
        </span>
      </div>
      
      <div class="mt-5">
        <v-btn
          color="primary"
          elevation="2"
          @click="generatePizza"
        >Make a Pizza</v-btn>
      </div>

      <hr class="mt-5 mb-5">

      <v-card>
        <v-list-item
          v-for="topping in randToppings"
          :key="topping.name"
        >{{ topping.name }}
        </v-list-item>
      </v-card>
    </v-card>
  </v-container>
</template>

<script>
import _ from 'lodash'

export default {
  data() {
    return {
      randToppings: [],
      toppingsQty: 1,
      toppings: [
        {name: "Pepperoni", category: "meat", active: true},
        {name: "Italian Sausage", category: "meat", active: true},
        {name: "Ham", category: "meat", active: true},
        {name: "Bacon", category: "meat", active: true},
        {name: "Smoked Chicken", category: "meat", active: true},
        {name: "Anchovies", category: "meat", active: true},
        {name: "Meatballs", category: "meat", active: true},
        {name: "Green Peppers", category: "veggie", active: true},
        {name: "Onions", category: "veggie", active: true},
        {name: "Tomatoes", category: "veggie", active: true},
        {name: "Spinach", category: "veggie", active: true},
        {name: "Green Olives", category: "veggie", active: true},
        {name: "Black Olives", category: "veggie", active: true},
        {name: "Mushrooms", category: "veggie", active: true},
        {name: "Garlic", category: "veggie", active: true},
        {name: "Banana Peppers", category: "veggie", active: true},
        {name: "Jalapenos", category: "veggie", active: true},
        {name: "Broccoli", category: "veggie", active: true},
        {name: "Basil", category: "veggie", active: true},
        {name: "Calobrian Peppers", category: "veggie", active: true},
        {name: "Pineapple", category: "veggie", active: true},
        {name: "Cherry Peppers", category: "veggie", active: true},
        {name: "Kalamata Olives", category: "veggie", active: true},
        {name: "Artichoke Hearts", category: "veggie", active: true},
        {name: "Roasted Red Peppers", category: "veggie", active: true},
        {name: "Feta", category: "cheese", active: true},
        {name: "Extra Mozzarella", category: "cheese", active: true},
        {name: "Dal", category: "cheese", active: true},
      ],
    }
  },

  methods: {
    generatePizza() {
      this.randToppings = []
      // Clone the toppings array
      var toppings2 = this.toppings.map(a => ({...a}))

      // TODO Filter it
      
      // Shuffle it
      this.shuffleArray(toppings2)

      // Draw X values where X is toppings quantity
      if (this.toppingsQty == 'Whatever') {
        this.toppingsQty = Math.floor(Math.random() * 6) + 1
      }

      for (var i = 0; i < this.toppingsQty; i++) {
        this.randToppings.push(toppings2[i])
      }
    },

    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    },

    toggleAll(array, status) {
      _.forEach(array, (topping) => {
        topping.active = status
      })
    },
  },

  computed: {
    meatToppings() {
      return _.sortBy(this.toppings.filter(topping => topping.category == 'meat'), 'name')
    },

    veggieToppings() {
      return _.sortBy(this.toppings.filter(topping => topping.category == 'veggie'), 'name')
    },

    cheeseToppings() {
      return _.sortBy(this.toppings.filter(topping => topping.category == 'cheese'), 'name')
    },

    isValid() {
      return false
      // TODO
      // Check that at least one topping is selected
    },
  }
}
</script>

<style scoped>
.wrap {
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
  padding: 24px;
}
</style>