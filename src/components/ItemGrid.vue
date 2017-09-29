//A Component for drinks grid
<template>
    <div id="drinkgrid" class="row row-content">
        <div v-for="drink in itemlist" class="col-xs-12 col-sm-6">
                <div class="media">
                    <div id="img" class="media-left media-middle">
                        <a href="#">
                            <img class="media-object img-thumbnail" v-bind:src="'./src/'+drink.src">
                        </a>
                    </div>
                    <div class="media-body">
                        <h3 id="dName" class="media-heading">{{drink.name}} <span id="dPrice" class="badge">${{drink.price }}</span></h3>
                        <div v-for="modifier in drink.modifiers" style="margin-bottom: 5px">
                            <label class="checkbox-inline">
                                <input type="checkbox" name="modifier" value="" v-model="modifier.select">
                                <strong id="mName">{{modifier.name}}   </strong><span id="mPrice" class="badge"> ${{ modifier.price }}</span>
                            </label>
                        </div>
                        <p><a class="btn btn-success btn-s" @click="add(drink)">Add to Order</a></p>
                    </div>
                </div>
                </div>
            </div>
</template>
<script>
    export default {
        props: {
            itemlist: {
                type: Array,
                default: () => []
            },
            orderlist: {
                type: Array,
                default: () => []
            }
        },
        methods: {
            add(drink) {
                var code = drink.code;
                var found = false;
                if (drink.modifiers.length)
                    for (var i = 0; i <= drink.modifiers.length - 1; i++)
                        if (drink.modifiers[i].select)
                            if (i == 0) code += 10;
                            else if (i == 1) code += 1;

                for (var item in this.orderlist) {
                    if (this.orderlist[item].code == code) {
                        found = true;
                        this.orderlist[item].quantity += 1
                    }
                }
                // Structure for each product with 2 methods for editing modifiers
                var item = function(name, price, code) {
                    this.name = name;
                    this.price = price;
                    this.quantity = 1;
                    this.code = code;
                    this.modifiers = Array();
                }
                item.prototype.addModifier = function(name, price) {
                    this.modifiers.push({
                        name,
                        price
                    });
                }
                item.prototype.removeModifier = function(index) {
                    this.modifiers.splice(index, 1);
                }
                item.prototype.addCount = function() {
                    this.count += 1;
                }
                item.prototype.orderQuantity = 0
                if (found == false) {
                    var myitem = new item(drink.name, drink.price, code);
                    for (var mod in drink.modifiers)
                        if (drink.modifiers[mod].select)
                            myitem.addModifier(drink.modifiers[mod].name, drink.modifiers[mod].price);
                    this.orderlist.push(myitem)
                }
            }
        }
    }

</script>
<style lang="scss">
    .media {
        padding: 25px 25px 25px 25px
    }

</style>
