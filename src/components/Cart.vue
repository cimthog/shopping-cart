<template>

  <div class="cart">
   
<div class="input-group mb-5">
  <div class="input-group-prepend">
    <span class="input-group-text">Product ID</span>
  </div>
  <textarea class="form-control" aria-label="Product ID" v-model="product.product_id"></textarea>
</div>

    <div class="input-group mb-5">
  <div class="input-group-prepend">
    <span class="input-group-text">Product Name</span>
  </div>
  <textarea class="form-control" aria-label="Product Name" v-model="product.product_name"></textarea>
</div>

  <div class="input-group mb-5">
  <div class="input-group-prepend">
    <span class="input-group-text">Product Quantity</span>
  </div>
  <textarea class="form-control" aria-label="Product Quantity" v-model="product.product_quantity"></textarea>
</div>

  <div class="input-group mb-5">
  <div class="input-group-prepend">
    <span class="input-group-text">Price: #</span>
  </div>
  <input type="text" class="form-control" aria-label="Amount (to the nearest naira)" v-model="product.product_price">
  <div class="input-group-append">
    <span class="input-group-text">.00</span>
  </div>
</div>

    <button class="btn btn-primary" id="addBtn" @click="addProduct">Add Product</button>

    <table class="table">
      <h4 class="table-title">Available Products</h4>
        <tr class="headoftable">
            <th scope="col">ID</th>
            <th scope="col">Description</th>
            <th scope="col">Quantity</th>
            <th scope="col">Price</th>
            <th scope="col">Actions</th>
        </tr>
        <tr v-for="item in cart.items" class="headoftable" :key="index">
          <td>{{item.product_id}}</td>
          <td>{{item.product_name}}</td>
          <td>{{item.product_quantity}}</td>
          <td>{{item.product_price}}</td>
          <button class="btn btn-danger" id="btnRemov" @click="removeItem(item)">Remove</button>
          <button class="btn btn-success" @click="addToCart">Add to Cart</button>
        </tr>
    </table>

    <table>
      <h4 class="table-title">Your Shopping Cart</h4>
        <tr class="headoftable">
            <th scope="col">ID</th>
            <th scope="col">Description</th>
            <th scope="col">Quantity</th>
            <th scope="col">Price</th>
            <th scope="col">Actions</th>
        </tr>
       
        <template v-if="checkout">
           <tr v-for="item in shopCart" class="headoftable" :key="row">
          <td>{{item.product_id}}</td>
          <td>{{item.product_name}}</td>
          <td>{{item.product_quantity}}</td>
          <td>{{item.product_price}}</td>
          <button class="btn btn-danger" @click="removeFromCart(item)">Delete</button>
        </tr>
        </template>
       
    </table>


    <div class="total" v-if="checkout"><b>Grand Total: {{setTotal()}}</b></div>
  </div>
</template>

<script>
export default {

  data () {
    return {
      checkout: false,
       cart: {
            items: []
        },
       product: {
         product_id:'',
         product_name:'',
         product_quantity:'',
         product_price:''
       },
       shopCart:[]
    }
     
  },
  methods:{
     addProduct: function() {
       if(this.product.product_id === "" || this.product.product_name === "" || this.product.product_quantity === "" || this.product.product_price === ''){
         alert("Please Fill all Inputs");
         return;
       }
       this.cart.items.push(this.product); 
       this.checkout = false;      
       
      },
      removeItem: function () {
         let index = this.cart.items.indexOf(this.cart.items);
        /*Removes the product from the listOfItems array which st
        ores all the items in the shopping cart */
        this.cart.items.splice(index, 1); //
      },
      addToCart: function() {
        this.shopCart.push(this.product);
        this.checkout = true;
        this.setTotal();   
      },
      removeFromCart: function () {
         let row = this.shopCart.indexOf(this.cart.items);
        /*Removes the product from the listOfItems array which st
        ores all the items in the shopping cart */
        this.shopCart.splice(row, 1); //
        this.setTotal();
      },
      setTotal: function () {
        return this.cartTotal;
      }

    },
    
    computed: {
        cartTotal: function() {
            var total = 0;
        
            this.shopCart.forEach(function(item) {
                total += item.product_quantity * item.product_price;
            });

            return total;
        },
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cart{
  margin-top: 40px;
  margin-left:10%;
  margin-right: 10%;
}
.table-titles{
    text-align: center;
    margin-top: 20px;
}
.tinput{
  width: 100%;
  margin-top: 10px;
  height: 32px;
  padding-left: 15px;
  font-size: 15px;
}
#addBtn{
  margin-top:40px;
}
table{
  margin-top: 15px;
}
.headoftable{

  background-color: gray;
  color: white;
  height: 40px;
}
h4{
  text-align: center;
  color: gray;
  display: inline-block;
  font-size: 20px;
}
th{
  width: 300px;
}
.total{
  font-size: 30px;
  margin-top: 50px;
  color: #000000;
}
#btnRemov{
  text-align: center;

  margin-right: 10px;
}
</style>
