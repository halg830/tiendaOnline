<script setup>
import { ref } from 'vue'
import controlXbox360 from "/src/imgs/control_xbox_360.png"
import xbox360 from "/src/imgs/consola 360.png"
import xboxOne from "/src/imgs/xboxOne.webp"
import playstation1 from "/src/imgs/playstation2.png"
import playstation5 from "/src/imgs/playstation5.jpg"

const convertirMoneda=(valor)=>{
  return valor.toLocaleString('es-CO', {
    style: 'currency',
    currency: 'COP'
  });
}

const convertirFloat=(valor)=>{
  return parseInt(valor.replace(/[^0-9-]/g, ''))/100;
}


const articulos = ref([
  {
    img: controlXbox360,
    nombre: "Control xbox 360",
    tienda: "Random",
    precio: convertirMoneda(90000)
  },
  {
    img: xbox360,
    nombre: "Xbox 360",
    tienda: "Random",
    precio: convertirMoneda(500000)
  },
  {
    img: playstation1,
    nombre: "Playstation 1",
    tienda: "Random",
    precio: convertirMoneda(200000)
  },
  {
    img: playstation5,
    nombre: "Playstation 5",
    tienda: "Random",
    precio: convertirMoneda(2000000)
  },
  {
    img: xboxOne,
    nombre: "Xbox One",
    tienda: "Random",
    precio: convertirMoneda(2000000)
  },
])


let estado = ref({display: "none"});

const carrito = ref([

]);

const abrirCar = () => {
  estado.value.display = estado.value.display==="none" ? "table" : "none";
};

let activarBt = ref(false)

const agregar = (item) => {
  const buscarItem = carrito.value.find(e => e.nombre == item.nombre)

  let evento = event.target 

  evento.textContent = "✔"
    setTimeout(() => {
      evento.textContent = "🛒"
    }, 1000);

  if (buscarItem) {
    
    buscarItem.cantidad += 1
    activarBt.value = false
    return
  }

  carrito.value.push({
    img: item.img,
    nombre: item.nombre,
    precio: convertirFloat(item.precio),
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



const aumentar = (i) => {
  activarBt.value = false

  carrito.value[i].cantidad += 1
}

const disminuir = (i) => {
  if (carrito.value[i].cantidad == 1) {
    activarBt.value = true
    return
  }

  carrito.value[i].cantidad -= 1
}

const vaciarCar = () => {
  carrito.value = []
}

</script>

<template>
  <div id="contBody">
    <!-- Barra superior -->
    <div id="barraTop">
      <h1>Mundo Gamer🎮</h1>
      <button class="iconocarro" @click="abrirCar()">🛒</button>
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
            <button @click="disminuir(index)" >➖</button>
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

    <div class="fondo">
      <div>hola
      </div>
    </div>
    

    <!-- Catalogo -->
    <div id="body">
      <h1>Nuestros artículos</h1>
      <div id="cont">
        <div class="card" v-for="(item, i) in articulos" :key="i">
          <img class="imgArt" :src="item.img" alt="">
          <div>
            <h4>{{ item.nombre }}</h4>
            <p>
              <b>Tienda: </b>
              {{ item.tienda }}
            </p>
            <p>
              <b>Precio: </b>
              {{ item.precio }}
            </p>
            <button @click="agregar(item)" class="btAgregar">🛒</button>

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* ContBody */
#contBody {
  background-image: url("/src/imgs/fondo.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  min-height: 100vh;
}

/* Catalogo */
.card {

  display: flex;
  flex-direction: row;
  align-items: center;
  height: 320px;
  width: 450px;
  padding: 20px;
  background-color: white;
  border-radius: 6px;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, .12);
  font-size: x-large;
}

.card>* {
  margin: 10px;
}

.btAgregar{

  padding: 10px;
  font-size: large;
}

.imgArt {
  width: 200px;
  height: 150px;
  margin-right: 20px;
}

#trBottom>* {
  border-top: 1px solid black;
}

#body {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100%;
 
}

#body>h1 {
  color: white;
  font-size: xx-large;
  box-shadow: 0px 0px 10px aqua;
  width: 100%;
  text-align: center;
}


#cont {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  gap: 15px;
  row-gap: 40px;
 margin-bottom: 20px;
 padding: 20px;
}

/* Carrito */
table {
  position: fixed;
  top: 80px;
  right: 10px;
  border: 1px solid black;
  background-color: rgb(181, 181, 181);
  text-align: center;
  border-collapse: collapse;
  z-index: 5000;
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
  position: fixed;
  top: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(45deg, #7a008feb, #001785c7);
  box-shadow: 5px 5px 30px rgb(255 0 247);
  color: white;
  text-shadow: 0 0 5px white;
  padding: 0 20px;
  z-index: 5000;
  width: 98%;
  height: 80px;
}


/* #barraTop>button {
  width: 40px;
  height: 40px;

} */

.fondo {
 
  margin-top: 80px;
  display: flex;
  height: 500px;
  align-items: center;
  justify-content: space-around;
  background-image: url(https://img1.wallspic.com/crops/5/9/5/8/3/138595/138595-monitor_de_computadora_de_pantalla_plana_negra_con_teclado_y_mouse-3840x2160.jpg);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}
/* .fondo::before {
  content: "";
  position: absolute;
  width: 100%;
  height: 500px;
  background-image: url(https://img1.wallspic.com/crops/5/9/5/8/3/138595/138595-monitor_de_computadora_de_pantalla_plana_negra_con_teclado_y_mouse-3840x2160.jpg);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  opacity: 0.3;
  z-index: -1;
} */
.iconocarro{
  height: 70px;
  width: 70px;
  font-size: 37px;
  background-color: transparent;
  border: 0px;
  color: black;
}

.iconocarro:hover{
  background-color: #001dab;
}

.carVisible{
  display: block;
}

.carInvisible{
  display: none;
}
</style>
