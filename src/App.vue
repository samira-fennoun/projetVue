<template>
        <!-- Navigation-->
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
            <div class="container px-4 px-lg-5">
                <a class="navbar-brand" href="#">My Store</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation"><span class="navbar-toggler-icon"></span></button>
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav me-auto mb-2 mb-lg-0 ms-lg-4">
                        <li class="nav-item">
                          <router-link class="nav-link active" aria-current="page" to="/">Home</router-link>
                        </li>
                        <li class="nav-item">
                          <router-link class="nav-link" to="/about">About</router-link>
                        </li>
                        <li class="nav-item">
                          <router-link class="nav-link" to="/new-product">New Product</router-link>
                        </li>
                    </ul>
                    <form class="d-flex">
                        <button class="btn btn-outline-dark" type="button" @click="toggleSideBar">
                            <i class="bi-cart-fill me-1"></i>
                            Cart
                            <span class="badge bg-dark text-white ms-1 rounded-pill"> {{ totalQuantity }}</span>
                        </button>
                    </form>
                </div>
            </div>
        </nav>
        <!-- Header-->
        <header class="bg-dark py-5">
            <div class="container px-4 px-lg-5 my-5">
                <div class="text-center text-white">
                    <h1 class="display-4 fw-bolder">Shop in style</h1>
                    <p class="lead fw-normal text-white-50 mb-0">With this shop home page template</p>
                </div>
            </div>
        </header>
        <Router-view
        :inventory = "inventory"
        :addTo = "addToCart"
        :addInv = "addInventory"
        />
        <Sidebar
        v-if="showSideBar"
        :toggle = "toggleSideBar"
        :cart = "cart"
        :inventory = "inventory"
        :remove = "removeItem"
        />
</template>

<script>
import Sidebar from '@/components/SideBar.vue'
import inventory from '@/product.json'

export default {
  components: {
    Sidebar
  },
  data: () => {
    return {
      showSideBar: false,
      inventory: inventory,
      cart: {}
    }
  },
  methods: {
    toggleSideBar () {
      this.showSideBar = !this.showSideBar
    },
    addToCart (product, index) {
      if (!this.cart[product]) this.cart[product] = 0
      this.cart[product] += inventory[index].quantity
      this.inventory[index].quantity = 0
    },
    removeItem (name) {
      delete this.cart[name]
    },
    addInventory (product) {
      this.inventory.push(product)
    }
  },
  computed: {
    totalQuantity () {
      return Object.values(this.cart).reduce((acc, curr) => {
        return acc + curr
      }, 0)
    }
  }
}
</script>
