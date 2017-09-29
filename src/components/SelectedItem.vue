<template>
          <div class="col-xs-12 col-sm-10 col-sm-offset-1">
            <div class="row row-content">
                <div class="col-xs-12">
                    <h3>Order List</h3>
                        <table class="table table-striped">
                            <tr>
                                <th>
                                    <h4>Drink</h4>
                                </th>
                                <th>
                                    <h4>Modifier</h4>
                                </th>
                                <th>
                                    <h4>Quantity</h4>
                                </th>
                                <th>&nbsp;</th>
                            </tr>
                            <tr id="itemrow" v-for="(item , index) in this.list">
                                <td id="cell"><b>{{item.name}}</b></td>
                                <td><span v-for="mod in item.modifiers">
                                     {{mod.name}}</span></td>
                                     <th>
                                         {{item.quantity}}
                                     </th>
                                <td>
                                    <p><button type="button" class="close btn-sm" aria-label="Close" v-on:click="delrow(index)"><span aria-hidden="true">&times;</span></button></p>
                                </td>
                            </tr>
                        </table>
                    </div>
            </div>
          </div>
  </template>

<script>
    export default {
        props: {
            list: {
                type: Array,
                default: () => []
            },
            orderQuantity: null
        },
        methods: {
            delrow: function(index) {
                if (this.list[index].quantity > 1)
                    this.list[index].quantity -= 1;
                else
                    this.list.splice(index, 1);
            }
        }
    }

</script>
<style lang="scss">
    #itemrow {
        border-top: 1px solid gray
    }
    
    #cell {
        height: 35px
    }

</style>
