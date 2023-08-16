<script setup>
import { ref } from 'vue'

const  dataImgs = {
  controlXbox360: "/src/imgs/control_xbox_360.png"
}

const articulos = ref([
  {
    img: dataImgs.controlXbox360,
    nombre: "Control xbox 360",
    tienda: "Random",
    precio: 90000
  },
  {
    img: "/src/imgs/control_xbox_360.png",
    nombre: "Control xbox 360",
    tienda: "Random",
    precio: 90000
  },
  {
    img: "/src/imgs/control_xbox_360.png",
    nombre: "Control xbox 360",
    tienda: "Random",
    precio: 90000
  },
  {
    img: "/src/imgs/control_xbox_360.png",
    nombre: "Control xbox 360",
    tienda: "Random",
    precio: 90000
  },
])


let estado = ref({ display: 'none' });

const carrito = ref([

]);

const abrirCar = () => {
  estado.value.display = estado.value.display === 'none' ? 'block' : 'none';
};

let activarBt = ref(false) 

const agregar = (item) => {
  const buscarItem = carrito.value.find(e => e.nombre == item.nombre)

  if (buscarItem) {
    buscarItem.cantidad += 1
    activarBt.value = false
    return
  }

  carrito.value.push({
    img: item.img,
    nombre: item.nombre,
    precio: item.precio,
    cantidad: 1,
  })
}

const calcularTotal = () => {
  if (carrito.value.length == 0) return 0
  return carrito.value.reduce((acumulador, actual) => {
    return acumulador + actual.precio * actual.cantidad
  }, 0)
}

const eliminar = (i) => {
  carrito.value.splice(i, 1)
}



const aumentar = (i)=>{
  activarBt.value = false

  carrito.value[i].cantidad+=1
}

const disminuir = (i)=>{
  if(carrito.value[i].cantidad==1){
    activarBt.value = true
     return
  }

  carrito.value[i].cantidad-=1
}

const vaciarCar = ()=>{
  carrito.value = []
}

</script>

<template>
  <div>
    <!-- Barra superior -->
    <div id="barraTop">
      <h1>Mundo Gamer🎮</h1>
      <button @click="abrirCar">🛒</button>
    </div>

    <!-- Carrito -->
    <div>
      <table :style="estado">
        <tr>
          <td>Imagen</td>
          <td>Nombre</td>
          <td>Precio</td>
          <td>Cantidad</td>
          <td></td>
        </tr>

        <tr v-for="(item, index) in carrito" :key="index" v-if="carrito.length > 0">
          <td><img class="imgCar" :src="item.img" alt=""></td>
          <td>{{ item.nombre }}</td>
          <td>{{ item.precio }}</td>
          <td>
            <button @click="disminuir(index)" :disabled="activarBt">➖</button>
            {{ item.cantidad }}
            <button @click="aumentar(index)">➕</button>
          </td>
          <td>

            <button @click="eliminar(i)">❌</button>
          </td>
        </tr>

        <tr v-else>
          <td style="text-align: center; column-span: 4;">No hay item en el carrito.</td>
        </tr>

        <tr id="trBottom">
          <td><button @click="vaciarCar">Vaciar carrito</button></td>
          <td></td>
          <td>Total: </td>
          <td>{{ calcularTotal() }}</td>
        </tr>

      </table>
    </div>

    <!-- Catalogo -->
    <div id="body">
      <h1>Consolas de videojuegos</h1>
      <div id="cont">
        <div class="card" v-for="(item, i) in articulos" :key="i">
          <img class="imgArt" :src="item.img" alt="">
          <h4>{{ item.nombre }}</h4>

          <p>
            <b>Tienda: </b>
            {{ item.tienda }}
          </p>
          <p>
            <b>Precio: </b>
            {{ item.precio }}
          </p>
          <button @click="agregar(item)">Agregar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Carrito */
.card {
  border: 1px solid black;
  display: flex;
  flex-direction: column;
  width: fit-content;
  padding: 20px;
}

.imgArt {
  width: 150px;
}

#trBottom>* {
  border-top: 1px solid black;
}

/* Catalogo */

#cont {
  display: grid;
  grid-template-columns: repeat(4, 25%);
}

table {
  position: absolute;
  right: 10px;
  border: 1px solid black;
  background-color: rgb(181, 181, 181);
  text-align: center;
  border-collapse: collapse;

}

td {
  border-bottom: 1px solid black;
  padding: 15px;
}

.imgCar {
  width: 80px;
}

/* Barra superior */

#barraTop {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: black;
  color: white;
  padding: 0 20px;
}

#barraTop>button {
  width: 40px;
  height: 40px;

}
</style>
