<template>
  <v-app>
      <Header :carts="carts" />

      <v-alert
          v-if="isAlert"
          elevation="5"
          border="bottom"
          dismissible
          :type="alertType"
          class="floatingAlert"
          :style="alertDismissible ? 'display:block' : 'display:none'" >{{ alertText }}</v-alert>
      <Products
          :products="products"
          @liked="likeProduct"
          @cart="addToCart"/>

      <Cart :carts="carts" :cartTotal="cartTotal"
            @deleteCartItem="deleteCartItem"
            @deleteAllItem="deleteCarts"
            @incrementCart="incrementCartItem($event)"
            @minusCart="minusCartItem($event)"/>
  </v-app>
</template>

<script>
import axios from "axios";
import Products from "./components/Products";
import Cart from "./components/Cart";
import Header from "./components/Header";

export default {
  name: 'App',

  components: {
      Header,
      Cart,
      Products,
  },

  data: () => ({
      products: [],
      carts: [],
      cartTotal: 0,
      isAlert: false,
      alertType: '',
      alertText: '',
      alertDismissible: false

  }),
    methods: {
        likeProduct: function (index) {
            this.isAlert = false;
            this.alertType = "";
            this.alertText = "";
            this.alertDismissible = false;
            if (this.products[index].like) {
                this.products[index].like = false;
                this.isAlert = true;
                this.alertType = "warning";
                this.alertText = "This product removed from favorites lists";
                this.alertDismissible = true;
            } else {
                this.products[index].like = true;
                this.isAlert = true;
                this.alertType = "success";
                this.alertText = "This product added from favorites lists";
                this.alertDismissible = true;
            }
            setTimeout(() => {this.isAlert = false}, 3000);
        },
        addToCart: function (product) {
            let checkNotExist = false;
            let self = this;
            this.carts.forEach(function (value) {
                if (value.name.toLowerCase().match(product.name.toLowerCase())) {
                    checkNotExist = true;
                    value.count ++;
                    self.cartTotal += (value.currentPrice * 1);
                }
            })
            if (! checkNotExist) {
                this.carts.push({
                    name: product.name,
                    image: product.image,
                    currentPrice: product.currentPrice,
                    date: product.date,
                    previousPrice: product.previousPrice,
                    count: 1,
                });
                self.cartTotal += (product.currentPrice * 1);
            }

        },
        incrementCartItem: function (cartIndex) {
            this.carts[cartIndex].count ++;
            this.cartTotal += (this.carts[cartIndex].currentPrice * 1);
        },
        minusCartItem: function (cartIndex) {
            if (this.carts[cartIndex].count > 1) {
                this.carts[cartIndex].count --;
                this.cartTotal -= (this.carts[cartIndex].currentPrice * 1);
            }
        },
        deleteCartItem: function (cartIndex) {
            if (cartIndex || cartIndex === 0) {
                let count = this.carts[cartIndex].count;
                let currentPrice = this.carts[cartIndex].currentPrice;
                this.carts.splice(cartIndex, 1);
                this.cartTotal -= (count * currentPrice);
            }
        },
        deleteCarts: function () {
            this.carts = [];
            this.cartTotal = 0;
        }
    },
    mounted() {
        axios
            .get("./data/products.json")
            .then(response => {this.products = response.data});
    }
};
</script>
<style>
.floatingAlert {
    position: fixed !important;
    left: 20px;
    top: 70px;
    z-index: 1111;
}
</style>