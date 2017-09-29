<template>
            <div class="col-xs-12 col-sm-10 col-sm-offset-1">
                <div id="facts" class="row row-content">
                    <div class="col-xs-12">
                        <h2>Your Invoice</h2>
                        <div class="table-responsive">
                            <table class="table table-striped">
                                <tr>
                                    <th>
                                        <h4>Drink and Modifier</h4>
                                    </th>
                                    <th>&nbsp;</th>
                                    <th>Quantity</th>
                                    <th>
                                        <h4>Price</h4>
                                    </th>
                                </tr>
                                <tr class="itemrow" v-for="item in this.list">
                                    <td id="cell"><b>{{item.name}}</b><span v-for="mod in item.modifiers">
                                         + {{mod.name}}</span></td>
                                    <td>&nbsp;</td>
                                    <td>{{item.quantity}}</td>
                                    <td>${{(item.finalPrice*item.quantity).toFixed(2)}}</td>
                                </tr>
                                <tr class="itemrow">
                                    <th>&nbsp;</th>
                                    <th>&nbsp;</th>
                                    <th>Subtotal</th>
                                    <th>${{this.Subtotal.toFixed(2)}}
                                    </th>
                                </tr>
                                <tr>
                                    <th>&nbsp;</th>
                                    <th>&nbsp;</th>
                                    <th>HST</th>
                                    <th>${{(this.Subtotal*0.13).toFixed(2)}}
                                    </th>
                                </tr>
                                <tr>
                                    <th>&nbsp;</th>
                                    <th>&nbsp;</th>
                                    <th>
                                        <h4>Total</h4>
                                    </th>
                                    <th>
                                        <h4>${{(this.Subtotal*1.13).toFixed(2)}}
                                        </h4>
                                    </th>
                                </tr>
                            </table>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" onClick="window.location='index.html'">Cancel</button>
                    <button type="button" v-on:click="checkout" class="btn btn-success">Checkout</button>
                </div>
        </div>
</template>
<script>
    export default {
        props: {
            list: {
                type: Array,
                default: () => []
            }
        },
        data() {
            return {
                Subtotal: 0
            }
        },
        methods: {
            calSub() {
                for (var d_item in this.list) {
                    var mPrice = 0;
                    for (var m_item in this.list[d_item].modifiers)
                        mPrice += this.list[d_item].modifiers[m_item].price;
                    this.list[d_item].finalPrice = this.list[d_item].price + mPrice;
                    this.Subtotal += this.list[d_item].finalPrice;
                }
            },
            checkout() {
                var invoice = {}
                invoice.id = Math.floor(Math.random() * Date.now())
                invoice.date = new Date(Date.now()).toLocaleString()
                invoice.itemList = this.list
                invoice.Subtotal = this.Subtotal
                invoice.Total = (this.Subtotal * 1.13).toFixed(2)
                console.log("Sent invoice :")
                console.log(invoice);
                fetch(`http://localhost:3000/invoices`, {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json'
                        },
                        body: JSON.stringify(invoice)
                    })
                    .then(function(res) {
                        return res.json()
                    })
                    .then(function(data) {
                        alert("An invoice with amount of $ " + (invoice.Subtotal * 1.13).toFixed(2) + " has processed successfully and sent to the json server!")
                    }).then(function() {
                        window.open('http://localhost:3000/invoices')
                    }).then(function() {
                        window.location = 'index.html'
                    })

            }
        },
        created() {
            this.calSub()
        }
    }

</script>
<style lang="scss">
    .itemrow {
        border-top: 1px solid gray
    }
    
    #cell {
        height: 35px
    }

</style>
