<template>
  <div class="d-flex">
    <b-modal id="modal-1" title="Producto agregado" hide-footer size="lg">
      <div>
        <b-card title="" :img-src="dataModal.dataProducto.photo" img-alt="Image" img-top tag="article" style="max-width: 80%" class="mb-2 centrarCard" img-left>
          <b-card-text>
            <ul>
              <li><b>Nombre Producto:</b> {{ dataModal.dataProducto.name }}</li>
              <li><b>Código Producto:</b> {{ dataModal.dataProducto.code }}</li>
              <li><b>Precio Producto:</b> ${{ dataModal.dataProducto.price }}.</li>
              <li>
                <div>
                  <div class="d-flex mb-2">
                    <b class="margenCantidad">Cantidad:</b>
                    <button @click="minus" :disabled="dataModal.cantidadProductos <= 0">
                      <b-iconstack font-scale="1">
                        <b-icon stacked icon="cart-dash" variant=""></b-icon>
                      </b-iconstack>
                    </button>
                    <input class="quantity w-25" min="0" name="quantity" v-model="dataModal.cantidadProductos" type="number" readonly />
                    <button @click="plus" :disabled="dataModal.cantidadProductos >= dataModal.dataProducto.stock - cantidadEnCarrito">
                      <b-iconstack font-scale="1">
                        <b-icon stacked icon="cart-plus" variant=""></b-icon>
                      </b-iconstack>
                    </button>
                  </div>
                </div>
              </li>
              <li>
                <b>Sub-Total: </b>
                <span> ${{ subTotalData > 0 ? subTotalData : subTotal }}</span>
              </li>
              <li class="mt-2">
                <b>Descripción del producto:</b>
                <p>
                  {{ dataModal.dataProducto.abstract }}
                </p>
              </li>
            </ul>
          </b-card-text>
        </b-card>
        <div class="d-flex justify-content-between w-75 centrarCard">
          <b-button class="mt-3" block @click="$bvModal.hide('modal-1')">Seguir comprando</b-button>
          <b-button class="mt-3" block @click="setDataCarrito" variant="success" :disabled="dataModal.cantidadProductos <= 0">Agregar al carro</b-button>
        </div>
      </div>
    </b-modal>
  </div>
</template>

<script>
import { mapGetters, mapActions, mapState } from "vuex";

export default {
  data() {
    return {
      subTotalData: 0,
    };
  },
  computed: {
    ...mapState("Carrito", {
      productosEnCarrito: "arrayProductos",
    }),
    ...mapGetters("Modal", {
      dataModal: "dataModalGet",
    }),
    subTotal() {
      return this.dataModal.cantidadProductos * this.dataModal.dataProducto.price;
    },
    cantidadEnCarrito() {
      let data = 0;
      if (this.productosEnCarrito.length > 0) {
        for (let i = 0; i < this.productosEnCarrito.length; i++) {
          if (this.productosEnCarrito[i].dataModal.dataProducto.id == this.dataModal.dataProducto.id) {
            data = this.productosEnCarrito[i].dataModal.cantidadProductos;
          }
        }
      }

      return data;
    },
  },
  methods: {
    minus() {
      this.dataModal.cantidadProductos--;
      if (this.dataModal.cantidadProductos < 0) {
        this.dataModal.cantidadProductos = 0;
      }
      this.subTotalResult();
    },
    plus() {
      this.dataModal.cantidadProductos++;
      this.subTotalResult();
    },
    subTotalResult() {
      this.subTotalData = this.dataModal.cantidadProductos * this.dataModal.dataProducto.price;
    },
    ...mapActions("Carrito", {
      addDataCarritoAction: "addDataCarritoAction",
    }),
    setDataCarrito() {
      this.subTotalComplete = this.subTotalData > 0 ? this.subTotalData : this.subTotal;
      this.addDataCarritoAction({
        dataModal: this.dataModal,
        subTotal: this.subTotalComplete,
      });
      this.$bvModal.hide("modal-1");
    },
  },
};
</script>

<style lang="css" scoped>
.centrarCard {
  margin-left: auto;
  margin-right: auto;
}
.card-img-top {
  max-width: 25rem;
  max-height: 25rem;
}
.margenCantidad {
  margin-right: 10px;
}
</style>
