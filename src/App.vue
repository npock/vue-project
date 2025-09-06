<script setup>
import { computed, reactive } from 'vue'

const columns = reactive({
  products_details: {
    name: 'Product Details',
  },
  price: { name: 'Price' },
  quantity: { name: 'Quantity' },
  subtotal: { name: 'Subtotal' },
  action: { name: 'Action' },
})

const defaultBascet = [
  {
    id: 1,
    products_details: {
      name: 'Blue Flower Print Crop Top',
      color: 'Yellow',
      size: 'M',
    },
    price: 29.0,
    quantity: 1,
    imageUrl: '../basket-initial-code/assets/crop-top.png',
  },
  {
    id: 2,
    products_details: { name: 'Levender Hoodie', color: 'Levender', size: 'XXL' },

    price: 119.0,
    quantity: 1,
    imageUrl: '../basket-initial-code/assets/hoodie.png',
  },
  {
    id: 3,
    products_details: { name: 'Black Sweatshirt', color: 'Black', size: 'XXL' },

    price: 123.0,
    quantity: 1,
    imageUrl: '../basket-initial-code/assets/sweatshirt.png',
  },
]

let localBascet = localStorage.getItem('total')
console.log(JSON.parse(localBascet))
if (localBascet === null || JSON.parse(localBascet).length === 0) {
  //localStorage.setItem('total', JSON.stringify(defaultBascet))
  localBascet = defaultBascet
} else {
  localBascet = JSON.parse(localBascet)
}

const basket = reactive(localBascet)

const increaseItemQuantity = (item) => {
  item.quantity++
  localStorage.setItem('total', JSON.stringify(basket))
}

const decreaseItemQuantity = (item) => {
  if (item.quantity > 1) {
    item.quantity--
  }
  localStorage.setItem('total', JSON.stringify(basket))
}

const removeItem = (id) => {
  const index = basket.findIndex((item) => item.id === id)
  basket.splice(index, 1)
  localStorage.setItem('total', JSON.stringify(basket))
}

const totalPrice = computed(() => {
  const sum = basket.reduce((a, b) => a + b.price * b.quantity, 0)

  return sum
})
</script>

<template>
  <body>
    <div class="container basket">
      <table class="basket-table">
        <thead class="basket-table__header">
          <tr>
            <th v-for="(value, key) in columns" :key="key">
              {{ key }}
            </th>
          </tr>
        </thead>
        <tbody class="basket-table__body">
          <tr v-for="item in basket" :key="item.id">
            <td v-for="(value, key) in columns" :key="value">
              <div class="basket-item__info" v-if="key === 'products_details'">
                <div class="basket-item__image">
                  <img :src="item.imageUrl" alt="" />
                </div>
                <h2 class="basket-item__info-h2">{{ item[key].name }}</h2>
                <p class="basket-item__info-p">Color:{{ item[key].color }}</p>
                <p class="basket-item__info-p">Size: {{ item[key].size }}</p>
              </div>
              <div class="basket-item__price" v-if="key === 'price'">${{ item[key] }}.00</div>
              <div class="basket-item__quantity" v-if="key === 'quantity'">
                <button @click="decreaseItemQuantity(item)" class="quantity-button">–</button>
                <input type="number" :value="item[key]" min="1" inputmode="numeric" />
                <button @click="increaseItemQuantity(item)" class="quantity-button">+</button>
              </div>
              <div class="basket-item__price" v-if="key === 'subtotal'">
                ${{ item.price * item.quantity }}.00
              </div>
              <div v-if="key === 'action'">
                <button @click="removeItem(item.id)" class="btn btn-delete" aria-label="Удалить">
                  <svg
                    class="w-6 h-6 text-gray-800 dark:text-white"
                    aria-hidden="true"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    fill="none"
                    viewBox="0 0 24 24"
                  >
                    <path
                      stroke="currentColor"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M5 7h14m-9 3v8m4-8v8M10 3h4a1 1 0 0 1 1 1v3H9V4a1 1 0 0 1 1-1ZM6 7h12v13a1 1 0 0 1-1 1H7a1 1 0 0 1-1-1V7Z"
                    />
                  </svg>
                </button>
              </div>
            </td>
          </tr>

          <tr v-if="basket.length === 0">
            <td colspan="5">
              <p class="basket-table__empty">No items</p>
            </td>
          </tr>

          <tr>
            <td colspan="5">
              <div class="basket-table__summary">
                <p class="basket-table__total">
                  Total <b>${{ totalPrice }}.00</b>
                </p>
                <p>Tax ${{ (totalPrice * 0.1).toFixed(2) }}</p>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </body>
</template>

<style src="./App.css"></style>
