<template>
  <div >
    <b-container >

    <b-row>
      <div class="selectparent">
    

      <div class="selectdiv bg-danger" variant="danger"> <div class="selectdiv2"><h2 class="selecth2">Flipkart</h2>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<input type="text" value="clothi" class="LM6RPg" title="Search for products, brands and more" name="q" autocomplete="off" placeholder="Search for products, brands and more">
</form>
<button class="vh79eN" type="submit"><svg width="20" height="20" viewBox="0 0 17 18" class="" xmlns="http://www.w3.org/2000/svg"><g fill="#2874F1" fill-rule="evenodd"><path class="_2BhAHa" d="m11.618 9.897l4.225 4.212c.092.092.101.232.02.313l-1.465 1.46c-.081.081-.221.072-.314-.02l-4.216-4.203"></path><path class="_2BhAHa" d="m6.486 10.901c-2.42 0-4.381-1.956-4.381-4.368 0-2.413 1.961-4.369 4.381-4.369 2.42 0 4.381 1.956 4.381 4.369 0 2.413-1.961 4.368-4.381 4.368m0-10.835c-3.582 0-6.486 2.895-6.486 6.467 0 3.572 2.904 6.467 6.486 6.467 3.582 0 6.486-2.895 6.486-6.467 0-3.572-2.904-6.467-6.486-6.467"></path></g></svg></button>
   </div>  
      </div>
      
    
    </div>
    <select class="categories" v-model="selectedCategory" v-on:change="categoryChange()">
        <option disabled value="">Product-categories</option>
        <option value="Clothing">Clothing</option>
        <option value="Baggage">Baggage</option>
        <option value="Footware">Footware</option>
        <option value="all">Show all</option>
      </select>
  </b-row>

<b-row class="shopcontainer">
  <b-col>
  <div class="products">

    <div class="shopcard" v-for="product in products" :key="product.id" v-if="product.show">
       <b-card
    :title= 'product.name'
    :img-src= "require(`@/assets/product${product.id}.jpeg`)"
    img-alt="Image"
    tag="article"
    style="max-width: 15rem;"
    class="mb-200"
  >
    <b-card-text>
      title: {{product.name}} <br>
      Price: €{{product.price}}
    </b-card-text>
   
    <b-button v-if="!product.cart" @click="add(product)"  href="#" variant="danger">Add to Cart</b-button>
    <b-button class="wish" v-if="!product.cart" @click="add(product)"  href="#" variant="danger">WishList </b-button>
     <b-button v-if="product.cart" @click="add(product)" :disabled="product.cart" href="#" variant="warning">Product added to Shopping Cart </b-button>
  </b-card>
    </div>
  </div>
  
  </b-col>
  </b-row>
  

  <b-row>
    <b-col>
      <h2>Shopping Cart</h2>
    </b-col>
  </b-row>


  <b-row>
    <b-col>
     <b-table bordered  hover :items="cart" :fields="fields">
       
      
          <template slot="#" slot-scope="data" >
       {{ data.index+1}}
      </template>
        <template slot="price" slot-scope="data" >
       {{ data.item.price * data.item.quantity}}
      </template>
       <template slot="remove" slot-scope="data" >
        <b-button @click="remove(data.item.id)" variant="danger"  class="mr-2">
          X
        </b-button>
      </template>
      <template slot="quantity" slot-scope="data">
        <b-row>
          <b-col cols="5">
             <b-button :disabled="data.item.quantity <=1" variant="danger" @click="decrement(data.item.id)"  class="mr-2">
          -
             </b-button>
          </b-col>
          <b-col cols="2">
            <h4>{{data.item.quantity}}</h4>
          </b-col>
          <b-col cols="5">
              <b-button variant="danger" @click="increment(data.item.id)" class="mr-2">
            +
        </b-button>
          </b-col>
        </b-row>
       
        
      
      </template>

      <template slot="image" slot-scope="data">
          <b-img style="max-width: 4rem;" :src= "require(`@/assets/product${data.item.id}.jpeg`)" fluid alt="Responsive image"></b-img>
       
      </template>
      
     </b-table>

    </b-col>
  </b-row>
  <b-row v-if="cart.length > 0">
     <b-col></b-col>
     <b-col></b-col>
     <b-col></b-col>
     <b-col></b-col>
     <b-col><h3>Total</h3></b-col>
     <b-col><h3>€ {{ total }}.00</h3></b-col>
  </b-row>
  <b-row v-if="cart.length > 0">
     <b-col>
       <b-button @click="clean" variant="info" block class="mr-2 mb-4">
          Clear
        </b-button>
     </b-col>
    <b-col></b-col>
     <b-col cols="4">Flipkart</b-col>
    
     <b-col>
        
     </b-col>
     <b-col>
        <b-button    @click="buy" variant="success" block class="mr-2">
          Buy
        </b-button>
     </b-col>
  </b-row>
   <b-modal hide-header-close no-close-on-esc no-close-on-backdrop ref="modal-1" centered title="Purchase Completed ">
     <template slot="modal-footer">
       <b-button class="mt-3" variant="info" block @click="clean">Close</b-button>

       
    </template>
    <p  class="my-4">Products:</p>
    <ul v-for="productFinal in ticket.products" :key="productFinal.id">
      <li >
       Product name: {{ productFinal.name}}
      </li>
       <li >
       Quantity: {{ productFinal.quantity }} 
      </li>
       <li >
       Price: {{ productFinal.price }}  
      </li>
       <li >
       Total: {{ productFinal.price * productFinal.quantity }}
      </li>
      <hr>
    </ul>
    <h2 class="">Total: € {{ticket.total}}.00</h2>
   
    
  </b-modal>
  
  </b-container>
   
  </div>
</template>

<script>

export default {
  name: 'Cart',
  props: {
    msg: String
  },
  data(){
    return {
      
      selectedCategory: "",
      ticket:{
        products: null,
        total:0
      },
      counter: 0,
      products: [
    {
        id:1,
        name:'banarasi saree',
        price:1,
        cart:false,
        quantity:1,
        category: "Clothing",
        show:true
    },
    {
        id:2,
        name:'kanjiwaram saree',
        price:2,
        cart:false,
        quantity:1,
        category: "Clothing",
        show:true
    },
    {
        id:3,
        name:'silk saree',
        price:3,
        cart:false,
        quantity:1,
        category: "Clothing",
        show:true
    },
     {
        id:4,
        name:' yellow Saree',
        price:10,
        cart:false,
        quantity:1,
        category: "Clothing",
        show:true
    },
   {
        id:5,
        name:'Lehenga',
        price:20,
        cart:false,
        quantity:1,
        category: "Clothing",
        show:true
    },
    {
        id:6,
        name:'Lehenga',
        price:30,
        cart:false,
        quantity:1,
        category: "Clothing",
        show:true
    },
    {
        id:7,
        name:'Lehenga',
        price:5,
        cart:false,
        quantity:1,
        category: "Clothing",
        show:true
    },
    {
        id:8,
        name:'Lehenga',
        price:7,
        cart:false,
        quantity:1,
        category: "Baggage",
        show:true
    },
  {
        id:9,
        name:'Womens black boot',
        price:15,
        cart:false,
        quantity:1,
        category: "Footware",
        show:true
    },
    {
        id:10,
        name:'Womens black sandals',
        price:15,
        cart:false,
        quantity:1,
        category: "Footware",
        show:true
    },
    {
        id:11,
        name:'Womens black sandals',
        price:15,
        cart:false,
        quantity:1,
        category: "Footware",
        show:true
    },
    {
        id:12,
        name:'Womens black heels sandals',
        price:15,
        cart:false,
        quantity:1,
        category: "Footware",
        show:true
    }
],
  cart:[],
  fields: ['#', 'remove', 'image','name','quantity','price']
    } // data return
  },
  methods: {
    categoryChange() {
        console.log(this.selectedCategory);
        this.products.forEach(element => {
          if (element.category == this.selectedCategory || this.selectedCategory == "all") element.show = true;
          else element.show = false;
        });
    },
     add(product){
       this.products[product.id-1].cart=true
       this.cart.push(product)
       this.counter++
     },
     clean(){
       this.cart=[];
      
       for (const key in this.products) {
          this.products[key].cart=false
          this.products[key].quantity=1
       }
       this.$refs['modal-1'].hide()
     },
    remove(id) {
      for (let index = 0; index < this.products.length; index++) {
       if (this.products[index].id == id) {
            this.products[index].cart=false
       } 
    }
    for (let index = 0; index < this.cart.length; index++) {
       if (this.cart[index].id == id) {
            this.cart.splice(index,1);
       } 
    }
    
},
buy(){
  this.ticket={
    products: this.cart,
    total: this.total
  }
  this.$refs['modal-1'].show()
  
}  ,
    increment(id){
      for (let index = 0; index < this.cart.length; index++) {
       if (this.cart[index].id == id) {
            this.cart[index].quantity++
       } 
    }
    },
    decrement(id){
      for (let index = 0; index < this.cart.length; index++) {
       if (this.cart[index].id == id) {
            this.cart[index].quantity--
       } 
    }
    }
  },
  computed: {
    total(){
      let t =0;
      for (let  index = 0; index < this.cart.length; index++) {
          t += this.cart[index].price*this.cart[index].quantity
      }
      return t
    }
  }
}
</script>

<style scoped>
.shopcontainer{
margin: 0 auto;
margin: 1vw 0vw 3vw 0vw;}

.products{ 
  padding: 0vw !important;
  display: flex;
  flex-wrap: wrap;
  justify-content:space-between; 
  }
.selectdiv{  
  display: inline;
  position: fixed;
  z-index: 2;
  margin-left: -50vw;
  margin-top: calc(-120px + 2vw);
  width: 100vw;
  padding: 1vw 0vw 1vw 0vw; 
 }
  
.selectdiv2{
margin-top: 0vw;  
width: 50vw;
z-index: 3;
display: absolute;
color:white;
text-align: left;
padding-left: 10vw;
font-family:Georgia, 'Times New Roman', Times, serif;
}

.shopcard{ 
margin: 0vw auto; 
margin-bottom: 2vw;
}
.selectparent{ 
width: 100vw;
height: 10vw;
}

.selecth2{ 
font-size: 5vw;


}
img:hover {
  opacity: 0.5;
  filter: alpha(opacity=50); /* For IE8 and earlier */
}
.wish{
  margin-left: 3px;
}
.LM6RPg {
    padding: 0 16px;
    border-radius: 2px 0 0 2px;
    border: 0;
    outline: 0 none;
    font-size: 14px;
    height: 36px;
    width: 100%;
}
.categories{
  margin-left: 0 auto 0;
  margin-top: 0 auto 0;
}
</style>
