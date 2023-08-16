<template>
    <div class="row">
         <my-defaultComponent></my-defaultComponent>
        <div class="col-12">
            <div class="card">
                <div class="card-header">
                    <h4>New Purchase</h4>
                </div>
                <div class="card-body">
                    <form @submit.prevent="create">
                        <div class="row">
                            <div class="col-3 mb-2">
                                <div class="form-group">
                                    <label>Date</label>
                                    <input type="date" class="form-control" >
                                </div>
                            </div>
                            <div class="col-3 mb-2">
                                <div class="form-group">
                                    <label>Invoice</label>
                                    <input type="text" class="form-control">
                                </div>
                            </div>
                            <div class="col-3 mb-2">
                                <div class="form-group">
                                    <label>Supplier</label>
                                    <input type="text" class="form-control" >
                                </div>
                            </div>
                            
                        </div>   
                        <div class="row">
                            <div class="col-12 mb-2">
                                <div class="form-group">
                                    <label>Product Name</label>
                                    <input type="text" class="form-control" v-model="keyword" placeholder="Product Name">
                                </div>
                            </div>                           
                        </div>     
                        <div class="row">
                            <div class="col-12 mb-2">
                                  <div class="card">
                                    <div class="card-header">                                     
                                    </div>
                                    <div class="card-body">
                                        <div class="table-responsive">
                                            <table class="table table-bordered">
                                                <thead>
                                                    <tr>
                                                        <th>SL</th>                                                    
                                                        <th>Product Name</th>                                                    
                                                        <th>Description</th>                                                      
                                                        <th>Quantity</th>
                                                        <th>Price</th>
                                                        <th>Total</th>
                                                        
                                                    </tr>
                                                </thead>
                                                <tbody v-if="products.length > 0">
                                                    <tr v-for="(product,key) in products" :key="key">
                                                        <td>{{ product.id }}</td>
                                                        <td>{{ product.name }}</td>
                                                        <td>{{ product.description }}</td>                                    
                                                        <td>
                                                              <input type="number" class="form-control" v-model="quantity">
                                                              <input type="hidden" class="form-control" id="product_id" v-model="product.id">
                                                        </td>                                    
                                                        <td>
                                                              <input type="number" class="form-control" v-model="price">
                                                        </td>                                    
                                                        <td>
                                                              <input type="number" class="form-control" v-model="total" readonly>
                                                        </td>                                    
                                                    </tr>
                                                </tbody>
                                                <tbody v-else>
                                                    <tr>
                                                        <td colspan="4" align="center">No Products Found.</td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                    </div>
                                </div>
                            </div> 
                        </div>     
                         <div class="col-3">
                                <button type="submit" class="btn btn-primary">Confirm</button>
                            </div>           
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import defaultComponent from '../layouts/Default.vue' 
export default {
    name:"productAdd",
    components:{
       'my-defaultComponent': defaultComponent,
    },
    data(){
        return {
            keyword: null,
            products: [],
            price: null,
            quantity:null,
            total: null,
            formData: {
                quantity:"",
                product_id:""
            }
        }
    },
    watch:{
        keyword(after,before) {
            this.getResults();
            
        },
        quantity(after,before){

            this.totalCal();
        },
        price(after,before){
            this.totalCal();
        }
    },
    methods:{
        async create(){
             this.formData.product_id = document.getElementById("product_id").value;
             this.formData.quantity = this.quantity;
            await axios.post('/api/product-purchase',this.formData).then(response=>{
                this.$router.push({name:"product"})
            }).catch(error=>{
                console.log(error)
            })
        },
        async getResults() {
           await axios.get('/api/livesearch', { params: { keyword: this.keyword } })
                .then(res => this.products = res.data)
                .catch(error => {});
        },
        async totalCal(){
          
                this.total = this.quantity*this.price;
        }
    }
}
</script>