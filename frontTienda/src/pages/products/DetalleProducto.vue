<template>
<q-page class="bg-grey-3 q-pa-md">
  <div class="row">
    <div class="col-12">
      <q-breadcrumbs>
        <q-breadcrumbs-el label="Inicio" to="/" />
        <q-breadcrumbs-el label="Productos" to="/" />
        <q-breadcrumbs-el :label="product?.nombre" />
      </q-breadcrumbs>
    </div>
    <div :class="`col-12 col-md-6 ${$q.screen.lt.md?'text-center':'text-right'}`">
      <q-img
        :src="product?.imagen.includes('http')?product?.imagen:`${$url}../images/${product?.imagen}`"
        spinner-color="white"
        v-if="product?.imagen"
        style="border: 2px solid #496aec;border-radius: 5px"
        width="250px"
        spinner-size="40px"></q-img>
    </div>
<!--    {-->
<!--    "id": 8,-->
<!--    "nombre": "HUGGIES ONE DONE REFRESHING TOALLITAS HUMEDAS X 48 UNIDADES",-->
<!--    "barra": null,-->
<!--    "cantidad": 0,-->
<!--    "cantidadAlmacen": 0,-->
<!--    "cantidadSucursal1": 0,-->
<!--    "cantidadSucursal2": 0,-->
<!--    "cantidadSucursal3": 0,-->
<!--    "cantidadSucursal4": 0,-->
<!--    "costo": 15.38,-->
<!--    "precioAntes": 29.5,-->
<!--    "precio": 20,-->
<!--    "activo": "ACTIVO",-->
<!--    "unidad": "PAQUETE",-->
<!--    "registroSanitario": null,-->
<!--    "paisOrigen": "",-->
<!--    "nombreComun": null,-->
<!--    "composicion": null,-->
<!--    "marca": "",-->
<!--    "distribuidora": "",-->
<!--    "imagen": "7702425800700_a34898b6-0419-4af4-ac82-6dcdfea028f7_543x543.webp",-->
<!--    "descripcion": "Las toallitas humedas Huggies naturally refreshing baby tienen un aroma a pepino fresco y te verde con una textura similar a una toallita. Estas toallitas no contienen alcohol y contienen ingredientes suaves como el aloe por lo que puede usar estas toallitas para limpiar mas que solo las nalgas de su bebe.",-->
<!--    "category_id": 6,-->
<!--    "agencia_id": 1,-->
<!--    "created_at": "2023-08-18T10:47:56.000000Z",-->
<!--    "updated_at": "2024-05-31T09:32:35.000000Z",-->
<!--    "subcategory_id": 24,-->
<!--    "cantidadSucursal5": 0,-->
<!--    "cantidadSucursal6": 0,-->
<!--    "cantidadSucursal7": 0,-->
<!--    "cantidadSucursal8": 0,-->
<!--    "cantidadSucursal9": 0,-->
<!--    "cantidadSucursal10": 0-->
<!--    }-->
    <div class="col-12 col-md-6">
      <div class="text-left q-px-md">
        <div class="text-h6">{{product?.nombre}}</div>
        <table>
          <tr>
            <td class="text-bold">Precio:</td>
            <td>
              Bs. {{product?.precio}}
              <q-badge v-if="es_porcentaje" color="red">
                Descuento {{product?.porcentaje}}%
                - Bs. {{(product?.precio * product?.porcentaje / 100).toFixed(2)}}
              </q-badge>
              <span>
                <q-badge color="green" v-if="es_porcentaje">
                  Bs. {{product?.precioNormal}}
                </q-badge>
              </span>
<!--              <pre>{{product}}</pre>-->
            </td>
          </tr>
          <tr>
            <td class="text-bold">Unidad:</td>
            <td>{{product?.unidad}}</td>
          </tr>
          <tr>
            <td class="text-bold">Stock:</td>
            <td>{{product?.cantidad > 100 ? '100' : product?.cantidad}}</td>
          </tr>
          <tr>
            <td class="text-bold">Principio activo:</td>
            <td>{{product?.composicion}}</td>
          </tr>
          <tr>
            <td class="text-bold">Distribuidora:</td>
            <td>{{product?.distribuidora}}</td>
          </tr>
          <tr>
            <td class="text-bold">Laboratorio:</td>
            <td>{{product?.marca}}</td>
          </tr>
          <tr>
            <td class="text-bold">Pais origen:</td>
            <td>{{product?.paisOrigen}}</td>
          </tr>
          <tr>
            <td class="text-bold">Registro sanitario:</td>
            <td>{{product?.registroSanitario}}</td>
          </tr>
        </table>
      </div>
      <div class="row">
        <div class="col-9 flex flex-center">
          <span class="text-bold">Cantidad:</span>
          <q-btn
            @click="cantidad = cantidad - 1"
            icon="remove"
            dense
            round
            color="primary"
            :disable="cantidad<=1"
          />
          <q-input v-model="cantidad" type="number"  min="1" class="text-center text-bold"  outlined dense style="text-align: center;width: 100px"/>
          <q-btn
            @click="cantidad = cantidad + 1"
            icon="add"
            dense
            round
            color="primary"
            :disable="cantidad>=product?.cantidad"
          />
          <span class="text-bold">{{product?.unidad}}</span>
        </div>
        <div class="col-3 bg-grey-4 text-right">
          <label class="text-bold ">Total</label>
          <div class="text-bold justify-between">
            Bs.
            <span>{{(product?.precio * cantidad).toFixed(2)}}</span>
          </div>
        </div>
        <div class="col-4 q-pa-xs">
          <q-btn
            @click="addCarrito(product, cantidad)"
            label="Añadir al carrito"
            icon="add_shopping_cart"
            class="full-width"
            color="blue"
            no-caps
            dense/>
        </div>
        <div class="col-4 q-pa-xs">
          <q-btn
            @click="addCart(product, cantidad, true)"
            label="Comprar ahora"
            icon="shopping_cart"
            class="full-width"
            color="red"
            no-caps
            dense/>
        </div>
        <div class="col-4 q-pa-xs">
          <q-btn
            @click="share"
            label="Compartir"
            icon="share"
            class="full-width"
            color="green"
            no-caps
            dense/>
        </div>
      </div>
      <div class="q-pt-xs">
        <div class="text-bold text-blue">Disponible en las siguientes sucursales:</div>
        <q-list dense>
          <template v-for="sucursal in sucursales" :key="sucursal.id">
          <q-item
            clickable
            v-if="sucursal.cantidad > 0"
          >
            <q-item-section avatar>
              <q-avatar>
                <q-icon name="store" />
              </q-avatar>
            </q-item-section>
            <q-item-section>
              <q-item-label>
                {{ sucursal.nombre }}
              </q-item-label>
              <q-item-label caption>
                {{ sucursal.direccion }}
              </q-item-label>
            </q-item-section>
            <q-item-section side>
              <q-badge color="green" :label="'Disponible'" />
            </q-item-section>
          </q-item>
          </template>
        </q-list>
      </div>
    </div>
    <div class="col-12">
      <label class="text-bold text-h6">Descripción:</label>
      <div class="text-justify q-px-md" v-html="product?.descripcion"></div>
    </div>
    <div class="col-12">
      <label class="text-bold text-h6">Productos relacionados:</label>
    </div>
  </div>
<!--  <pre>{{product}}</pre>-->
<!--  <pre>{{sucursales}}</pre>-->
</q-page>
</template>
<script>
export default {
  name: 'DetalleProducto',
  data () {
    return {
      id: this.$route.params.id,
      product: {},
      loading: true,
      cantidad: 1,
      es_porcentaje: false,
      sucursales: []
    }
  },
  async mounted () {
    await this.getSucursales()
    await this.getProduct()
  },
  methods: {
    async getSucursales () {
      const response = await this.$axios.get('sucursales')
      this.sucursales = response.data
    },
    async getProduct () {
      this.loading = true
      this.$q.loading.show()
      const response = await this.$axios.get(`productos/${this.id}`)
      this.product = response.data
      if (this.product.porcentaje > 0) {
        this.es_porcentaje = true
        this.product.precioNormal = this.product.precio
        const precio = this.product.precio - (this.product.precio * this.product.porcentaje / 100)
        this.product.precio = precio.toFixed(2)
      }
      this.sucursales.forEach(sucursal => {
        sucursal.cantidad = this.product[`cantidadSucursal${sucursal.id}`]
      })
      this.loading = false
      this.$q.loading.hide()
    },
    addCarrito (product, cantidad) {
      product.cantidad = cantidad
      this.$store.addCarrito(product)
      this.$alert.success('Producto añadido al carrito')
    },
    addCart (product, cantidad, comprar = false) {
      const text = `Deseo comprar ${cantidad} ${product.nombre} a Bs. ${product.precio} c/u. Total Bs. ${(product.precio * cantidad).toFixed(2)}`
      window.open(`https://wa.me/59172319869?text=${text}`)
    },
    share () {
      const shareData = {
        title: this.product.nombre,
        text: this.product.descripcion,
        url: window.location.href
      }
      navigator.share(shareData)
        .then(() => console.log('Successful share'))
        .catch((error) => console.log('Error sharing', error))
    }
  }
}
</script>
