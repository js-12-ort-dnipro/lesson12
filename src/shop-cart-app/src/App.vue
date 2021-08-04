<template>

  <div class='container d-flex flex-column min-vh-100'>   
    
    <header>

      <div class="d-flex align-items-center justify-content-between">
            
        <h1>Shop-App</h1>

        <cart-widget  
          @click='state.showCart = !state.showCart' 
          :items-quantity='itemsQuantityInCart' 
          :items-total-cost='itemsTotalCostInCart'
        />

      </div>

    </header>

    <main class='flex-grow-1'>

      <cart-component 
        v-show='state.showCart'
        :cart-list='state.cart'
        :remove-from-cart-method='removeFromCart'
        :items-total-cost='itemsTotalCostInCart'
        :closeCartMethod='closeCart' 
      />

      <div v-show='!state.showCart' class="row row-cols-1 row-cols-md-2 row-cols-lg-4">
          
        <one-product 
          v-for='item in state.products' 
          :key='item.id'
          :product-data="item" 
          :in-cart="inCart(item)"
          :add-to-cart="addToCart"  
        />

      </div>
      
    </main>

    <footer class='text-center'>

      <h5 class='my-4 text-secondary'>Shop-App &copy; 2021</h5>
    
    </footer>

  </div>

</template>

<script setup>
 
    const URL = 'https://fakestoreapi.com/products';

    import { defineProps, reactive, onMounted, computed } from 'vue';

    import OneProduct     from './components/OneProduct.vue';
    import CartWidget     from './components/CartWidget.vue';
    import CartComponent  from './components/CartComponent.vue';


    const state = reactive({
      products: [],
      cart: [],
      showCart: false,
    });

    function inCart(product){
      return state.cart.some(i => i.product.id === product.id);
    }

    function addToCart(product){

        let checkIsExist = state.cart.filter(i => i.product.id === product.id);

        if(checkIsExist.length){
          
          checkIsExist[0].quantity++;

        }else{

          state.cart.push({
            product: product,
            quantity: 1
          });

        }
    }

    function closeCart(){
        state.showCart = false;
      }

    function removeFromCart(cartItem){
      state.cart = state.cart.filter(i => i.product.id !== cartItem.product.id);
    }

    const itemsQuantityInCart = computed(() => state.cart.reduce((acc, i) => acc + i.quantity, 0));
    
    const itemsTotalCostInCart = computed(() => +state.cart.reduce((acc, i) => acc + i.product.price * i.quantity, 0).toFixed(2));
        

    onMounted(async () => {
        let data = await fetch(URL);
        
        data = await data.json();

        state.products = data;
    });

</script>

<style scoped>
  
</style>