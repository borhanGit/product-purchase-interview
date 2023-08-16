<template>
    <div class="row">
         <my-defaultComponent></my-defaultComponent>
        <div class="col-12">
            <div class="card">
                <div class="card-header">
                    <h4>Add Product</h4>
                </div>
                <div class="card-body">
                    <form @submit.prevent="create">
                        <div class="row">
                            <div class="col-12 mb-2">
                                <div class="form-group">
                                    <label>Name</label>
                                    <input type="text" class="form-control" v-model="product.name">
                                </div>
                            </div>
                            <div class="col-12 mb-2">
                                <div class="form-group">
                                    <label>Description</label>
                                    <input type="text" class="form-control" v-model="product.description">
                                </div>
                            </div>
                            <div class="col-12">
                                <button type="submit" class="btn btn-primary">Save</button>
                            </div>
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
            product:{
                name:"",
                description:""
            }
        }
    },
    methods:{
        async create(){
            await axios.post('/api/product',this.product).then(response=>{
                this.$router.push({name:"product"})
            }).catch(error=>{
                console.log(error)
            })
        }
    }
}
</script>