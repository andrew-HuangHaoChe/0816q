<template>
    <div>
        <div class="container">
            <div class="row mt-4">
                <div class="col-md-3">
                    <div class="list-group sticky-top">
                        <a href="#all" 
                            class="list-group-item list-group-item-action" :class="{ active: filter === 'all' }"
                            @click.prevent="filter='all'"
                            >
                            <i aria-hidden="true" class="far fa-keyboard mr-2"></i>全部鍵盤
                        </a>
                        <a href="#ducky" 
                            class="list-group-item list-group-item-action option-font" 
                            data-toggle="list"
                            :class="{ active: filter === 'ducky'}"
                            @click.prevent="filter ='ducky'">
                            <i aria-hidden="true" class="far fa-keyboard mr-2"></i>Ducky
                        </a>
                        <a href="#irock" 
                            class="list-group-item list-group-item-action option-font" 
                            data-toggle="list"
                            :class="{ active: filter === 'irock'}"
                            @click.prevent="filter = 'irock'">
                            <i aria-hidden="true" class="far fa-keyboard mr-2"></i>irock
                        </a>
                        <a href="#filco" 
                            class="list-group-item list-group-item-action option-font" 
                            data-toggle="list"
                            :class="{ active: filter === 'filco'}"
                            @click.prevent="filter = 'filco'">
                            <i aria-hidden="true" class="far fa-keyboard mr-2"></i>filco
                        </a>
                       
                    </div>
                </div>
                <div class="col-md-9">
                    <div class="row">
                    <div class="col-md-4 mb-4 box-shadow" v-for="item in filterProducts" :key="item.id">
                        <div class="card border-0 shadow-sm">
                        <div style="height: 150px; background-size: cover; background-position: center" :style="{ backgroundImage:`url(${item.imageUrl})` }"
                            >
                        </div>
                        <div class="card-body">
                            <span class="badge badge-secondary float-right ml-2">{{item.category}}</span>
                            <h5 class="card-title">
                            <a href="#" class="text-dark">{{item.title}}</a>
                            </h5>
                            <p class="card-text">{{item.content}}</p>
                            <div class="d-flex justify-content-between align-items-baseline">
                            <div class="h5" v-if="!item.price">{{item.origin_price}} 元</div>
                            <del class="h6">原價 {{item.origin_price}} 元</del>
                            <div class="h5">現在只要 {{item.price}} 元</div>
                            </div>
                        </div>
                        <div class="card-footer d-flex">
                            <button type="button" class="btn btn-outline-secondary btn-sm" @click="getproduct(item.id)">
                            <i class="fas fa-spinner fa-spin" v-if="status.loadingItem === item.id"></i>
                            查看更多
                            </button>
                            <button type="button" class="btn btn-outline-danger btn-sm ml-auto" @click="addtoCart(item.id)">
                            <i class="fas fa-spinner fa-spin" v-if="status.loadingItem === item.id"></i>
                            加到購物車
                            </button>
                        </div>
                        </div>
                        
                    </div>
                    
                </div>
                <Pagination :pages="pagination" @pageChange="getProducts"></Pagination>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import $ from 'jquery';
import Pagination from './Pagination';
export default{
    components:{
        Pagination,
    },
    data(){
        return{
        products:[],
        keyboardSort:[],
        pagination:{},
        isLoading:false,
        status:{
            loadingItem:'',
        },
        isActive:true,
        filter:'ducky',
        }
    },
    methods:{
        getProducts(page = 1){
            const api=`${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/products?page=${page}`;
            const vm = this;
            vm.isLoading = true;
            this.$http.get(api).then((response) => {
            console.log(response);
            vm.isLoading = false;
            vm.products = response.data.products;
            vm.pagination = response.data.pagination;
            vm.products.forEach(element => {
                vm.keyboardSort.push(element.category);
                });
            });
        },
    },
    computed:{
        filterProducts: function(){
            if(this.filter =='all'){
                return this.products;
             }
            else if(this.filter == 'ducky'){
                return this.products.filter(function(item,index,array){
                    return item.category ==='Ducky';
                });
            }else if(this.filter == 'irock'){
                return this.products.filter(function(item,index,array){
                    return item.category ==='i-rock';
                });
            }else if(this.filter == 'filco'){
                return this.products.filter(function(item,index,array){
                    return item.category ==='Filco';
                });
            }
        },
        // filterData: function(){
        //     const vm = this;
        //     return vm.keyboardSort.filter(function(element,index){
        //         return vm.keyboardSort.indexOf(element) === index;
        //     });
        // },
    },
    created() {
        this.getProducts();
    },

}
</script>