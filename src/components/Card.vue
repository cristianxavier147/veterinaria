<template>
  <b-card title="" :img-src="dataProducto.photo" img-alt="Image" img-top tag="article" style="max-width: 20rem" class="mb-2">
    <b-card-text>
      <ul>
        <li>
          <b>Stock:</b>
          {{ dataProducto.stock ? "Disponible" : "No Disponble" }}
        </li>
        <li>
          <b>Cantidad Stock:</b>
          {{ dataProducto.stock - this.cantidadEnCarrito }}.
        </li>
        <li><b>Nombre:</b> {{ dataProducto.name }}.</li>
        <li><b>Precio Producto:</b> ${{ dataProducto.price }}</li>
      </ul>
    </b-card-text>

    <div>
      <div class="d-flex justify-content-center mb-2">
        <button @click="minus" :disabled="cantidadProductos <= 0">
          <b-iconstack font-scale="1">
            <b-icon stacked icon="cart-dash" variant=""></b-icon>
          </b-iconstack>
        </button>
        <input class="quantity w-25" min="0" name="quantity" v-model="cantidadProductos" type="number" readonly />
        <button @click="plus" :disabled="cantidadProductos >= dataProducto.stock - cantidadEnCarrito">
          <b-iconstack font-scale="1">
            <b-icon stacked icon="cart-plus" variant=""></b-icon>
          </b-iconstack>
        </button>
      </div>

      <b-button class="w-100" variant="primary" @click="cargaModal" v-b-modal.modal-1 :disabled="cantidadProductos <= 0 || this.cantidadEnCarrito >= dataProducto.stock">
        <b-iconstack font-scale="1"> <b-icon stacked icon="cart-check" variant=""></b-icon></b-iconstack
      ></b-button>
    </div>
  </b-card>
</template>

<script>
import { mapActions, mapState } from "vuex";

export default {
  components: {},
  props: {
    dataProducto: {
      type: Object,
      default: () => {
        return {};
      },
    },
  },
  computed: {
    ...mapState("Carrito", {
      productosEnCarrito: "arrayProductos",
    }),
    cantidadEnCarrito() {
      let data = 0;
      if (this.productosEnCarrito.length > 0) {
        for (let i = 0; i < this.productosEnCarrito.length; i++) {
          if (this.productosEnCarrito[i].dataModal.dataProducto.id == this.dataProducto.id) {
            data = this.productosEnCarrito[i].dataModal.cantidadProductos;
          }
        }
      }

      return data;
    },
  },
  data() {
    return {
      cantidadProductos: this.cantidadEnCarrito ? this.dataProducto.stock - this.cantidadEnCarrito : 0,
    };
  },
  methods: {
    minus() {
      this.cantidadProductos--;
      if (this.cantidadProductos < 0) {
        this.cantidadProductos = 0;
      }
    },
    plus() {
      this.cantidadProductos++;
    },
    // en el method cargaModalestoy usando el mapAction para pasar parametros
    ...mapActions("Modal", {
      cargaDataModal: "cargaDataModalAction",
    }),
    cargaModal() {
      // Para pasar parametros desde un ...mapAction se hace de esta manera
      this.cargaDataModal({
        dataProducto: this.dataProducto,
        cantidadProductos: this.cantidadProductos,
      });
      setTimeout(() => {
        this.cantidadProductos = 0;
      }, 500);
    },
  },
};
</script>

<style lang="css">
ul {
  list-style-type: none;
  margin-left: -30px;
}
</style>
