<template>
    <div class="row">
        <my-defaultComponent></my-defaultComponent>
        <div class="col-12 mb-2 text-end">
            <router-link :to='{name:"productAdd"}' class="btn btn-primary">Create</router-link>
        </div>
        <div class="col-12">
            <div class="card">
                <div class="card-header">
                    <h4>Product</h4>
                </div>
                <div class="card-body">
                    <div class="table-responsive">
                        <table class="table table-bordered">
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Name</th>
                                    <th>Description</th>
                                    
                                </tr>
                            </thead>
                            <tbody v-if="products.length > 0">
                                <tr v-for="(product,key) in products" :key="key">
                                    <td>{{ product.id }}</td>
                                    <td>{{ product.name }}</td>
                                    <td>{{ product.description }}</td>                                    
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
</template>

<script>
import defaultComponent from '../layouts/Default.vue' 
export default {
    name:"products",
    components:{
       'my-defaultComponent': defaultComponent,
    },
    data(){
        return {
            products:[]
        }
    },
    mounted(){
        this.getProducts()
    },
    methods:{
        async getProducts(){
            await axios.get('/api/product').then(response=>{
                this.products = response.data
            }).catch(error=>{
                console.log(error)
                this.products = []
            })
        }
    }
}
</script>