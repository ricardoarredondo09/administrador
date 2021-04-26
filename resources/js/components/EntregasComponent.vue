<template>
<div class="container">
    <div class="row justify-content-center">
        <div class="col-md-12">
            <div class="card">
                <div class="card-header">Administrar Entregas</div>
                <div class="card-body">
                    <table class="table table-responsive">
                      <thead>
                        <tr>
                          <th scope="col">N° Pedido</th>
                          <th scope="col">Envia</th>
                          <th scope="col">Telefono</th>
                          <th scope="col">Fecha Entrega</th>
                          <th scope="col">Recibe</th>
                          <th scope="col">Direccion</th>
                          <th scope="col">Entregado</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="(item, index) in entregas" :key="index" :class="{'table-success': item.status === 'completed'}">
                          <th scope="row">
                            {{item.id}}
                          </th>
                            <td>
                                {{item.billing.first_name}} {{item.billing.last_name}}
                            </td>
                          <td>
                            <a href="tel:">{{item.billing.phone}}</a>
                          </td>

                          <td v-for="meta_data in item.meta_data" v-if="meta_data.key === 'fecha-de-entrega'">
                            {{meta_data.value}}
                          </td>

                          <td>
                            {{item.shipping.first_name}} {{item.shipping.last_name}}
                          </td>
                          <td>
                            {{item.shipping.address_1}}, {{item.shipping.state}}
                          </td>
                          <td>
                            <button  class="btn btn-primary" :class="{'disabled': item.status === 'completed'}" data-target="#modalConfirmar"  data-toggle="modal"  @click="modalEntregarPedido(index, item)" > Entregado</button>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
    <div class="modal fade" id="modalConfirmar" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Entregar Pedido</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="alert alert-success" role="alert">
              Pedido Entregado con Exito
            </div>
            <h3>¿Esta Seguro que desea entregar el pedido?</h3>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Cerrar</button>
            <button type="button" class="btn btn-primary" @click="entregarPedido()">Entregar</button>
          </div>
        </div>
      </div>
    </div>
</div>
</template>
<script>
    export default {
        data(){
            return{
                entregas: [],
                id: 0,
                orden:[]
            }
        },
        created(){
            axios.get('/entregas').then(res=> {
                this.entregas = res.data;
                console.log(this.entregas);
            })
        },
        methods: {
          entregarPedido(){
            axios.put(`/entregas/${this.orden.id}`)
           .then(res=>{

            this.entregas[this.id].status = 'completed';
          })
        },
        modalEntregarPedido(index, item)
        {
          this.orden = item;
          this.id= index;
        }
    }
  }
</script>