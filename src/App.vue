<template>
    <div id="app">
        <!-- ----------- Navbar  ------------- -->
        <nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                        aria-expanded="false" aria-controls="navbar">
                    <span class="sr-only">Toggle navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <a class="navbar-brand" href="Index.html">Cafe Vue</a>
            </div>
            <div id="navbar" class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li v-bind:class="actM">
                        <a @click="route('menu')">
                            <span class=" glyphicon glyphicon-list-alt" aria-hidden="true"></span> Menu</a>
                    </li>
                    <li v-bind:class="actP">
                        <a v-on:click="route('payment')"><span class="glyphicon glyphicon-shopping-cart"
                                                               aria-hidden="true"></span> Cart</a>
                    </li>
                    <li>
                        <a onclick="window.open( 'http://localhost:3000/invoices');"> # INVOICES #</a>
                    </li>
                </ul>
            </div>
        </nav>


        <!-- ---------Confirmation Modal ------------>
        <div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="order confirmation">
            <div class="modal-dialog modal-lg" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span
                                aria-hidden="true">&times;</span></button>
                        <h4 class="modal-title" id="myModalLabel">Payment Confirmation</h4>
                    </div>
                    <div class="modal-body">
                        <h4>The amount of ${{(Subtotal * 1.13).toFixed(2)}} has processed successfully! </h4>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-success" data-dismiss="modal"
                                onClick="window.location='index.html'">Close
                        </button>
                    </div>
                </div>
            </div>

        </div>

        <!-- ----------- Jumbotron  ------------- -->
        <header class="jumbotron">
            <div class="container">
                <div class="row row-header">
                    <div class="col-xs-12 col-sm-8">
                        <p style="padding:10px;"></p>
                        <img src="./images/CafeVue.png" class="img-responsive">
                    </div>
                    <div class="col-xs-12 col-sm-4">
                        <p style="padding:10px;"></p>
                        <p>A simple Point-Of-Sale application for a coffee shop via VueJS</p><a
                            class="btn btn-primary btn-md" v-on:click="route('payment')" v-if="menu">Payment</a>
                        <a class="btn btn-primary btn-md" v-on:click="route('menu')" v-if="payment">Home Page</a>
                    </div>
                </div>
            </div>
        </header>

        <!-- ----------- Drinks grid ------------- -->
        <ItemGrid :itemlist="products" :orderlist="cart" v-if="menu"></ItemGrid>

        <!-- ----------- Selected Drinks ----------- -->
        <SelectedItem :list="cart" v-if="(menu)&(cart.length>0)"></SelectedItem>

        <!-- -----------  Invoice  ----------- -->
        <Invoice :list="cart" v-if="payment"></Invoice>

        <div class="col-xs-12" v-if="(menu)&(cart.length>0)">
            <p align="center"><a class="btn btn-primary btn-md" v-on:click="route('payment')">Payment</a></p>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue';
    import ItemGrid from './components/ItemGrid.vue';
    import SelectedItem from './components/SelectedItem.vue';
    import Invoice from './components/Invoice.vue';
    // A closure for keeping items into the order list
    // function itemArray() {
    //     var list = Array();
    //     return {
    //         addItem: function (item) {
    //             list.push(item);
    //         },
    //         removeItem: function (index) {
    //             list.splice(index, 1);
    //         },
    //         getList: function () {
    //             return list; 
    //         },
    //         length: function () {
    //             return list.length;
    //         }
    //     }
    // };

    export default {
        name: 'app',
        data() {
            return {
                products: [],
                Subtotal: null,
                cart: [],
                payment: false,
                menu: true,
                actM: 'active',
                actP: ''
            }
        },
        components: {
            ItemGrid,
            SelectedItem,
            Invoice
        },
        methods: {
            fetchProducts() {
                fetch(`http://localhost:3000/drinks`, {
                    method: 'GET'
                })
                    .then(response => response.json())
                    .then(json => this.products = json)
            },
            route(page) {
                if (page == "payment") {
                    this.payment = true;
                    this.menu = false;
                    this.actM = '';
                    this.actP = 'active'
                } else {
                    this.payment = false;
                    this.menu = true;
                    this.actM = 'active';
                    this.actP = ''
                }

            }
        },
        created() {
            this.fetchProducts();
        }
    }

</script>


<style lang="scss">
    //fonts
    $font-stack: 'Avenir',
    Helvetica,
    Arial,
    sans-serif;
    // variables
    $vue-blue: rgb(53, 73, 94);
    $vue-green: rgb(65, 184, 131);
    // mixins
    @mixin border-radius($radius) {
        -webkit-border-radius: $radius;
        -moz-border-radius: $radius;
        -ms-border-radius: $radius;
        border-radius: $radius;
    }

    @mixin box-shadow($type, $top, $leftright, $bottom, $color) {
        -webkit-box-shadow: $type $top $leftright $bottom $color;
        -moz-box-shadow: $type $top $leftright $bottom $color;
        -ms-box-shadow: $type $top $leftright $bottom $color;
        box-shadow: $type $top $leftright $bottom $color;
    }

    #drinkgrid {
        background: $vue-blue;
        font-family: $font-stack;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: white;
    }

    .img-thumbnail {
        @include border-radius(20px);
        @include box-shadow(0, 11px, 6px, -4px, black);
    }

    .navbar-inverse {
        background: $vue-blue;
        color: #ffffff;
        cursor: pointer;
    }

    .navbar-inverse .navbar-nav > .active > a {
        background: $vue-green;
        color: #ffffff;
    }

    .navbar-inverse .navbar-nav > .active > a:hover {
        background: $vue-green;
        color: #ffffff;
    }

    .btn.btn-primary {
        background: $vue-blue;
        @include box-shadow(0, 10px, 6px, -6px, #777);
    }

    .btn.btn-success {
        background: $vue-green;
        @include box-shadow(0, 10px, 6px, -6px, black);
    }

    .btn.btn-default {
        @include box-shadow(0, 10px, 6px, -6px, black);
    }

    .badge {
        background: #ab8600;
        @include box-shadow(0, 8px, 6px, -6px, black);
    }

    .jumbotron {
        margin: 0;
        padding: 40px
    }

    .media-body {
        text-align: left;
        padding: 10px
    }

</style>
