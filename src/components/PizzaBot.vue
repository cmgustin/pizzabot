<template>
  <v-container>
    <v-card class="wrap">
      <div class="teal white--text pl-4 pr-4 pt-2 pb-1">
        <h1 style="font-size: 24px;">PizzaBot</h1>
        <p style="margin-bottom: 8px;"><small><em>The Automatic Pizza Maker</em></small></p>
      </div>

      <div class="wrap-inner">
        <div>
          <label class="d-block text-overline">How many toppings?</label>
          <v-select
            v-model="toppingsQty"
            :items="[1, 2, 3, 4, 5, 6]"
            label="How many toppings?"
            solo
          ></v-select>
        </div>

        <label class="d-block text-overline">Customize</label>
        <v-expansion-panels>
          <v-expansion-panel>
            <v-expansion-panel-header>
              <span>
                <strong class="mr-2">Meats</strong> 
                <small>{{ meatToppingsActive.length }} / {{ meatToppings.length }}</small>
              </span>
            </v-expansion-panel-header>
            <v-expansion-panel-content>
              <span 
                class="v-chip v-chip--clickable v-chip--no-color theme--light v-size--default mr-2 mb-4"
                :class="{'v-chip--active primary--text': topping.active}"
                v-for="topping in meatToppings"
                :key="topping.name"
                @click="topping.active = !topping.active"
              >
                {{ topping.name }}
              </span>

              <hr>
              <a @click="toggleAll(meatToppings, true)"><small>All</small></a> 
              <span> / </span>
              <a @click="toggleAll(meatToppings, false)"><small>None</small></a>
            </v-expansion-panel-content>
          </v-expansion-panel>

          <v-expansion-panel>
            <v-expansion-panel-header>
              <span>
                <strong class="mr-2">Veggies</strong> 
                <small>{{ veggieToppingsActive.length }} / {{ veggieToppings.length }}</small>
              </span>
            </v-expansion-panel-header>

            <v-expansion-panel-content>
              <span 
                class="v-chip v-chip--clickable v-chip--no-color theme--light v-size--default mr-2 mb-4"
                :class="{'v-chip--active primary--text': topping.active}"
                v-for="topping in veggieToppings"
                :key="topping.name"
                @click="topping.active = !topping.active"
              >
                {{ topping.name }}
              </span>

              <hr>
              <a @click="toggleAll(veggieToppings, true)"><small>All</small></a> 
              <span> / </span>
              <a @click="toggleAll(veggieToppings, false)"><small>None</small></a>            
            </v-expansion-panel-content>
          </v-expansion-panel>

          <v-expansion-panel class="mb-5">
            <v-expansion-panel-header>
              <span>
                <strong class="mr-2">Cheeses</strong> 
                <small>{{ cheeseToppingsActive.length }} / {{ cheeseToppings.length }}</small>
              </span>
            </v-expansion-panel-header>

            <v-expansion-panel-content>
              <span 
                class="v-chip v-chip--clickable v-chip--no-color theme--light v-size--default mr-2 mb-4"
                :class="{'v-chip--active primary--text': topping.active}"
                v-for="topping in cheeseToppings"
                :key="topping.name"
                @click="topping.active = !topping.active"
              >
                {{ topping.name }}
              </span>

              <hr>
              <a @click="toggleAll(cheeseToppings, true)"><small>All</small></a> 
              <span> / </span>
              <a @click="toggleAll(cheeseToppings, false)"><small>None</small></a>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
        
        <div class="form-footer mt-5">
          <v-btn
            @click="generatePizza"
            :disabled="!isValid"
            :loading="isLoading"
            rounded
            x-large
            outlined
            color="teal"
          >
            <img src="@/assets/food-icons/pizza-2.svg" alt="" width="40" class="mr-3">
            Make Pizza
          </v-btn>
            
          <div class="custom-error" v-if="!isValid">
            <small class="red--text">Not enough toppings selected</small>
          </div>
        </div>

        <v-divider class="mb-3"></v-divider>

        <label class="d-block text-overline">Your Pizza</label>
        <v-card ref="outputCard">
          <div v-if="isLoading">
            <v-skeleton-loader
              v-for="n in toppingsQty"
              :key="n"
              type="list-item-avatar, list-item-one-line"
              class="item-border-bottom"
            ></v-skeleton-loader>
          </div>

          <div v-else-if="isInit">
            <v-list-item style="height: 56px;">
            <em>Click the button to make your first pizza</em>
            </v-list-item> 
          </div>
          
          <div v-else>
            <v-list-item
              v-for="topping in randToppings"
              :key="topping.name"
              class="item-border-bottom"
            >
              <v-list-item-icon>
                <img :src="getToppingIcon(topping.icon)" alt="topping.name Icon">
              </v-list-item-icon>

              <v-list-item-content>
                {{ topping.name }}
              </v-list-item-content>
            </v-list-item>
          </div>
        </v-card>
      </div>

      <div class="footer">
        <small class="d-block">&copy; Copyright {{ currentYear }} Chris Gustin. All rights reserved.</small>
        <small class="d-block">Inspired by <a href="https://www.facebook.com/automaticpizzaathens/" target="_blank">Automatic Pizza</a> in Athens, GA</small>
        <div><small>Icons made by <a href="https://www.flaticon.com/authors/smashicons" title="Smashicons" target="_blank">Smashicons</a> from <a href="https://www.flaticon.com/" title="Flaticon" target="_blank">www.flaticon.com</a></small></div>
      </div>
    </v-card> 
  </v-container>
</template>

<script>
import _ from 'lodash'

export default {
  data() {
    return {
      isInit: true,
      isLoading: false,
      randToppings: [],
      toppingsQty: 1,
      toppings: [
        {name: "Pepperoni", category: "meat", active: true, icon: 'salami'},
        {name: "Italian Sausage", category: "meat", active: true, icon: 'sausage'},
        {name: "Ham", category: "meat", active: true, icon: 'ham'},
        {name: "Bacon", category: "meat", active: true, icon: 'bacon'},
        {name: "Smoked Chicken", category: "meat", active: true, icon: 'meat'},
        {name: "Anchovies", category: "meat", active: true, icon: 'fish'},
        {name: "Meatballs", category: "meat", active: true, icon: 'salami'},
        {name: "Green Peppers", category: "veggie", active: true, icon: 'pepper'},
        {name: "Onions", category: "veggie", active: true, icon: 'onion-1'},
        {name: "Tomatoes", category: "veggie", active: true, icon: 'tomato'},
        {name: "Spinach", category: "veggie", active: true, icon: 'salad-1'},
        {name: "Green Olives", category: "veggie", active: true, icon: 'olives'},
        {name: "Black Olives", category: "veggie", active: true, icon: 'seeds'},
        {name: "Mushrooms", category: "veggie", active: true, icon: 'mushrooms'},
        {name: "Garlic", category: "veggie", active: true, icon: 'garlic'},
        {name: "Banana Peppers", category: "veggie", active: true, icon: 'chili'},
        {name: "Jalapenos", category: "veggie", active: true, icon: 'chili'},
        {name: "Broccoli", category: "veggie", active: true, icon: 'broccoli'},
        {name: "Basil", category: "veggie", active: true, icon: 'salad-1'},
        {name: "Calobrian Peppers", category: "veggie", active: true, icon: 'chili'},
        {name: "Pineapple", category: "veggie", active: true, icon: 'pineapple'},
        {name: "Cherry Peppers", category: "veggie", active: true, icon: 'chili'},
        {name: "Kalamata Olives", category: "veggie", active: true, icon: 'seeds'},
        {name: "Artichoke Hearts", category: "veggie", active: true, icon: 'onion'},
        {name: "Roasted Red Peppers", category: "veggie", active: true, icon: 'chili'},
        {name: "Feta", category: "cheese", active: true, icon: 'cheese-1'},
        {name: "Extra Mozzarella", category: "cheese", active: true, icon: 'cheese-1'},
        {name: "Dal", category: "cheese", active: true, icon: 'cheese-1'},
      ],
    }
  },

  methods: {
    generatePizza() {
      // Reset variables
      this.isInit = false
      this.isLoading = true
      this.randToppings = []
      this.$refs.outputCard.$el.style.minHeight = '56px'

      // Shuffle
      this.shuffleArray(this.activeToppings)

      // Draw X values where X is toppings quantity
      setTimeout(() => {
        this.$refs.outputCard.$el.style.minHeight = this.$refs.outputCard.$el.clientHeight + 'px'
        console.log(this.$refs.outputCard.$el.style.minHeight)

        for (var i = 0; i < this.toppingsQty; i++) {
          this.randToppings.push(this.activeToppings[i])
        }

        this.isLoading = false
      }, 500)
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

    getToppingIcon(icon) {
      return require(`../assets/food-icons/${icon}.svg`)
    }
  },

  computed: {
    activeToppings() {
      return this.toppings.filter(topping => topping.active == true)
    },

    meatToppings() {
      return _.sortBy(this.toppings.filter(topping => topping.category == 'meat'), 'name')
    },

    veggieToppings() {
      return _.sortBy(this.toppings.filter(topping => topping.category == 'veggie'), 'name')
    },

    cheeseToppings() {
      return _.sortBy(this.toppings.filter(topping => topping.category == 'cheese'), 'name')
    },

    meatToppingsActive() {
      return this.toppings.filter(topping => topping.category == 'meat' && topping.active == true)
    },

    veggieToppingsActive() {
      return this.toppings.filter(topping => topping.category == 'veggie' && topping.active == true)
    },

    cheeseToppingsActive() {
      return this.toppings.filter(topping => topping.category == 'cheese' && topping.active == true)
    },

    isValid() {
      if (this.activeToppings.length == 0 || this.activeToppings.length < this.toppingsQty) {
        return false
      }
      
      return true
    },

    currentYear() {
      return new Date().getFullYear()
    }
  }
}
</script>

<style scoped>
.v-btn--outlined {
  border-width: 2px;
}

.wrap {
  display: flex;
  min-height: calc(100vh - 24px);
  flex-direction: column;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
}

.wrap-inner {
  flex: 1;
  padding: 24px;
}

.form-footer {
  position: relative;
  padding-bottom: 24px;
}

.form-footer .custom-error {
  position: absolute;
  bottom: 0;
  left: 0;
}

.v-skeleton-loader {
  padding-top: 4px;
  padding-bottom: 4px;
}

.item-border-bottom {
  border-bottom: 1px solid #ccc;
}

.item-border-bottom:last-child {
  border-bottom: none;
}

.footer {
  margin-top: 24px;
  padding: 16px;
  text-align: center;
  background: #f7f7f7;
}

@media screen and (max-width: 500px) {
  .wrap {
    min-height: 100vh;
    border-radius: 0 !important;
  }

  .container {
    padding: 0;
  }    
}
</style>