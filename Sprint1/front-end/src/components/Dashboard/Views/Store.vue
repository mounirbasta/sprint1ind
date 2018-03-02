<template>

<body>
<head>
 <title>Bootstrap Example</title>
 <meta charset="utf-8">
 <meta name="viewport" content="width=device-width, initial-scale=1">
 <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">

</head>

<div class="container" style = "background-color : #f4f3ef">

 <h1> Products </h1>

  <form id= "form" class="form-inline" action="#/admin/Store" >

    <h6> Add product</h6>

    <div class="form-group">
    <h4> Name <input type="text" class="form-control"  placeholder="Name" id="text" v-model="product.name" ></h4>
    </div>

    <div class="form-group">
    <h4>Seller Name <input type="text" class="form-control" placeholder="SellerName"  id="text" v-model="product.sellername"></h4>
    </div>

    <div class="form-group">
    <h4>Price <input type="number" class="form-control"  placeholder="Price"  min ='1' v-model="product.price" ></h4>
    </div>

    <button type="submit" class="btn btn-danger" @click="addToAPI()">Add product</button>

  </form>

  <form id= "form" class="form-inline" action="#/admin/Store" >
    <h6> Edit product</h6>
    <div class="form-group">
    <h4> Name <input type="text" class="form-control"  placeholder="Name" id="text" v-model="edit.name" ></h4>
    </div>

    <div class="form-group">
    <h4>Price <input type="number" class="form-control"  placeholder="Price"  min ='1' v-model="edit.price" ></h4>
    </div>
    <h6> "To edit please enter product name and price then choose the edit button adjacent: to update the desired row" </h6>
    <h1>  </h1>
  </form>

<h1></h1>
 <table class="table table-bordered">
   <thead>
     <tr>

       <th style="width: 15%" class= "text-center">ID</th>
       <th style="width: 15" class= "text-center">Name</th>
       <th style="width: 15" class= "text-center">Price</th>
       <th style="width: 15%" class= "text-center">Created at</th>
       <th style="width: 15%" class= "text-center">Updated at</th>
       <th style="width: 15%" class= "text-center">Seller Name</th>
       <th style="width: 13%" class= "text-center"><p> <span class= "glyphicon glyphicon-wrench"></span></p></th>

     </tr>
   </thead>
   <tbody>
     <tr v-for="product_proc in products.data">

        <td>{{ product_proc._id}}</td>
        <td>{{ product_proc.name}}</td>
        <td>{{ product_proc.price}}</td>
        <td>{{ product_proc.createdAt}}</td>
        <td>{{ product_proc.updatedAt}}</td>
        <td>{{ product_proc.sellername}}</td>
        <td>  <div class="btn-group">

        <button type="button" class="btn btn-success" @click="Edit(product_proc._id)">Edit</button>

         <button type="button" class="btn btn-danger" @click="deleteP(product_proc._id)">Delete</button>

        </div></td>

     </tr>

   </tbody>
 </table>

</div>

</body>

</template>

<script>

import axios from 'axios'

export default {
  name: 'product',
  data () {
    return {
      products: [],
      edit: [],
      product: {
        name: '',
        sellername: '',
        price: ''
      }
    }
  },
  created () {
    this.viewAdd()
  },
  methods: {
    addToAPI () {
      let newProduct = {
        name: this.product.name,
        sellername: this.product.sellername,
        price: this.product.price
      }
      axios.post('http://localhost:3000/api/product/createProduct', newProduct)
      .then((response) => {
        console.log(response)
        window.location.reload()
      })
      .catch((error) => {
        console.log(error)
      })
    },
    viewAdd () {
      var p = this
      axios.get('http://localhost:3000/api/product/getProducts')
      .then((response) => {
        this.products = response.data
        console.log(response.data)
      })
      .catch((error) => {
        console.log(error)
      })
    },
    deleteP (del) {
      axios.delete('http://localhost:3000/api/product/deleteProduct/' + del)
      .then((response) => {
        this.products.splice(index, 1)
      })
      .catch((error) => {
        console.log(error)
      })
      window.location.reload()
    },
    Edit (ed) {
      let newEdit = {
        name: this.edit.name,
        price: this.edit.price
      }
      axios.patch('http://localhost:3000/api/product/updateProduct/' + ed, newEdit)
      .then((response) => {
        console.log(response)
      })
      .catch((error) => {
        console.log(error)
      })
      window.location.reload()
    }
  }
}
</script>
