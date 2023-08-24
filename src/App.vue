<template>
  <div class="container-all">
    <header>
      <div class="arriba">
        <img src="/src/assets/logo.png" alt="">
        <button @click="mostrarCarrito" type="button" class="btn-carrito"><i class="fa-solid fa-cart-shopping" style="color: #ffffff;"></i></button>
        <div v-if="mostrarTablaCarrito" class="carrito">
          <div class="table-scroll">
            <table class="basic-table">
              <thead>
                  <tr>
                      <th>Vehiculo</th>
                      <th>Nombre</th>
                      <th>Precio</th>
                      <th>Cantidad</th>
                      <th>Precio por Cantidad</th>
                      <th class="vaciar-carrito"></th>
                  </tr>
              </thead>
              <tbody>
                  <tr v-for="(item, i) in carrito" :key="i">
                      <td><img :src=item.img alt="" class="imagen-tabla"></td>
                      <td>{{ item.nombre }}</td>
                      <td>${{ (item.precio_ahora).toLocaleString("es-ES", {
                          style: "currency",
                          currency: "COP",
                          minimumFractionDigits: 0,
                          maximumFractionDigits: 2,
                        })}}
                      </td>
                      <td>{{ item.cantidad }}</td>
                      <td>${{ (item.precio_ahora*item.cantidad).toLocaleString("es-ES", {
                          style: "currency",
                          currency: "COP",
                          minimumFractionDigits: 0,
                          maximumFractionDigits: 2,
                        }) }}
                      </td>
                      <td class="td-eliminar"><button  @click="eliminar(i)" class="eliminar">❌</button></td>
                  </tr> 
                  <div colspan="5" class="raya-carrito"></div> 
                  <tr>
                    <td colspan="4" class="total">Total:</td>
                    <td>${{calcularTotal().toLocaleString("es-ES", {
                        style: "currency",
                        currency: "COP",
                        minimumFractionDigits: 0,
                        maximumFractionDigits: 2,
                      }) }}
                      </td>
                    <td class="td-btn-vaciar-carrito"><button type="button" class="btn btn-danger btn-vaciar-carrito" @click="vaciarCarrito()">Vaciar</button></td>
                  </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div class="imagen-header">
        <img src="/src/assets/fondo.png" alt="">
      </div>
    </header>
    <div class="container-cards">
      <div v-for="(item, i) in data" :key="i" class="card">
        <div class="container-moto">
          <img id="imagenmoto" :src=item.img alt="">
          <h3 id="nombreMoto">{{ item.nombre }}</h3>
        </div>
        <div class="container-info">
          <div class="precios">
            <h4>Precio Antes:</h4>
            <h4>Precio Ahora:</h4>
          </div>
          <div class="valores">
            <h4>$ {{ (item.precio_antes).toLocaleString("es-ES", {
              style: "currency",
              currency: "COP",
              minimumFractionDigits: 0,
              maximumFractionDigits: 2,
            }) }}</h4>
            <div class="raya-precio"></div>
            <h4>$ {{ (item.precio_ahora).toLocaleString("es-ES", {
              style: "currency",
              currency: "COP",
              minimumFractionDigits: 0,
              maximumFractionDigits: 2,
            }) }}</h4>
          </div>
          
        </div>
        <div class="info">
          <div class="cilindraje">
            <h4>{{item.cilindrada}}</h4>
            <h4>Cilindraje</h4>
          </div>
          <div class="potencia">
            <h4>{{item.potencia}}</h4>
            <h4>Potencia</h4>
          </div>
          <div class="peso">
            <h4>{{item.peso}}</h4>
            <h4>Peso</h4>
          </div>
        </div> 
        <div class="btn">
          <button type="button" class="btn btn-primary" @click="agregarCarrito(item)">Agregar al carrito</button>      
        </div>    
      </div>
    </div>
    <footer>
      <h1>MOTOSPORTS</h1>
      <div class="regresar">
        <button onclick="window.location.href = 'index.html'" class="boton">Regresar</button>
    </div>
    </footer>
  </div>
</template>
<script setup>
import { ref } from 'vue'
import img1 from "/src/assets/BMW_S1000RR.png"
import img2 from "/src/assets/BMW-F-900-XR.png"
import img3 from "/src/assets/bmw-r-1250.png"
import img4 from "/src/assets/Duke_1290.png"
import img5 from "/src/assets/hayabusa.png"
import img6 from "/src/assets/Honda_crf_300.png"
import img7 from "/src/assets/Kawasaki_Ninja400KRT.png"
import img8 from "/src/assets/Kawasaki-z1000.png"
import img9 from "/src/assets/KTM-390-Duke.png"
import img10 from "/src/assets/panigale-v4-ducati.png"
import img11 from "/src/assets/Yamaha-R1.png"
import img12 from "/src/assets/YamahaR6.png"

let data = ref([
  { img: img1, nombre: "BMW S1000RR", precio_antes: 157990000 , precio_ahora: 145990000, cilindrada: "999cc", potencia: "210 cv", peso: "197 Kg" },
  { img: img2, nombre: "BMW F900XR", precio_antes: 92990000  , precio_ahora: 90990000 , cilindrada: "895cc", potencia: "105 cv", peso: "209 Kg" },
  { img: img3, nombre: "BMW 1250 GS", precio_antes: 164990000 , precio_ahora: 156990000, cilindrada: "1254cc", potencia: "136 cv", peso: "249 Kg" },
  { img: img4, nombre: "DUKE 1290", precio_antes: 129990000, precio_ahora: 115990000, cilindrada: "1.301cc", potencia: "177 cv", peso: "189 Kg" },
  { img: img5, nombre: "HAYABUSA", precio_antes: 101300000, precio_ahora: 95300000, cilindrada: "1.340cc", potencia: "190 cv", peso: "264 Kg" },
  { img: img6, nombre: "HONDA CRF300", precio_antes: 26990000, precio_ahora: 24990000, cilindrada: "286cc", potencia: "26.95 cv", peso: "142 Kg" },
  { img: img7, nombre: "KAWASAKI NINJA 400", precio_antes: 35990000, precio_ahora: 32990000, cilindrada: "399 cc", potencia: "48.3 cv", peso: "168 Kg" },
  { img: img8, nombre: "KAWASAKI Z1000", precio_antes: 72990000, precio_ahora: 67990000, cilindrada: "1043cc", potencia: "142 cv", peso: "221 Kg" },
  { img: img9, nombre: "KTM DUKE 390", precio_antes: 26990000, precio_ahora: 23990000, cilindrada: "373cc", potencia: "44 cv", peso: "150 Kg" },
  { img: img10, nombre: "PANIGALE V4 DUCATI", precio_antes: 116900000, precio_ahora: 111900000, cilindrada: "1103cc", potencia: "215.3 cv", peso: "195.5 Kg" },
  { img: img11, nombre: "YAMAHA R1", precio_antes: 100000000, precio_ahora: 95000000, cilindrada: "998cc", potencia: "200 cv", peso: "199 Kg" },
  { img: img12, nombre: "YAMAHA R6", precio_antes: 92000000, precio_ahora: 89000000, cilindrada: "599cc", potencia: "118 cv", peso: "169.6 Kg" },
])


let carrito =ref([]);

const agregarCarrito = (item) => {
  let carritoItem = carrito.value.find(cartItem => cartItem.nombre === item.nombre);
  if (carritoItem) {
    carritoItem.cantidad++;
  } else {
    carritoItem = { ...item, cantidad: 1 };
    carrito.value.push(carritoItem);
  }
}

let mostrarTablaCarrito = ref(false);

const mostrarCarrito = () => {
  mostrarTablaCarrito.value = !mostrarTablaCarrito.value;
}

const calcularTotal = () => {
  let total = 0;
  carrito.value.forEach(item => {
    total += item.precio_ahora*item.cantidad;
  });
  return total;
}
function eliminar(i) {
  carrito.value.splice(i, 1)
}
function vaciarCarrito(){
  carrito.value = [];
}

</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Monoton&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap');

header {
  height: 20%;
  width: 100%;
}

.arriba{
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: black;
  border: none;
}
.arriba button{
  display: flex;
  align-items: center;
  height: 30px;
  width: 35px;
  position: relative;
  right: 5%;
}
.arriba img{
  height: 200px;
  width: 200px;
  border: none;
}
.imagen-header{
  display: flex;
  width: 100%;
  height: 500px;
}
.imagen-header img{
  width: 100%;
}

.carrito{
  position: absolute;
  background-color: white;
  top: 16%;
  right: 5%;
  z-index: 2000;
  margin: 0;
  border-radius: 10px;

}

.btn-carrito{
  background-color: black;
  border: none;
  font-size: 20px;
}
.raya-carrito{
  margin: 2px 0px;
}
table {
  border-collapse: collapse;
  width: 100%;
  border-radius: 10px;
}
td .imagen-tabla{
  border-radius: 5px;
  height: 50px;
  width: 50px;
}
th, td {
  padding: 10px;
  text-align: center;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #f2f2f2;
  font-family: "Poppins", sans-serif;
}

tbody tr:nth-child(odd) {
  background-color: #f2f2f2;
}

td .eliminar{
  display: flex;
  justify-content: center;
  text-align: center;
  height: 40px;
  width: 40px;
  background-color: rgb(247, 151, 151);
  border-radius: 10px;
}
.eliminar{
  border: none;
}

.total{
  text-align: left;
  border: none;
}
.table-scroll {
  max-height: 329px; /* Ajusta esta altura según tus necesidades */
  overflow-y: auto;
}

.vaciar-carrito{
  width: 80px;
}

.td-eliminar{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 80px;
}
.td-btn-vaciar-carrito .btn-vaciar-carrito{
  width: 100%;
}

.container-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  justify-content: space-evenly;
  background-color: white;
  border: none;
}
.card {
  width: 400px;
  height: 650px;
  margin: 20px;
  background-color: white;
  -webkit-box-shadow: 5px 5px 18px -4px rgba(122,122,122,1);
  -moz-box-shadow: 5px 5px 18px -4px rgba(122,122,122,1);
  box-shadow: 5px 5px 18px -4px rgba(122,122,122,1);
  border-radius: 10px;
}
.container-moto {
  position: absolute;
  width: 100%;
  height: 55%;
  backface-visibility: hidden;
  border-radius: 10px;
  overflow: hidden;
}

.container-moto  img {
  position: absolute;
  width: 400px;
  object-fit: cover;
}
.container-moto h3 {
  position: absolute;
  top: 85%;
  width: 100%;
  height: 45px;
  line-height: 45px;
  color: #fff;
  background: rgba(0, 0, 0, .4);
  text-align: center;
}

.agregar-carrito{
  background-color: rgb(71, 194, 243);
  border-radius: 10px;
  margin-top: 5px;
}

.container-info{
  position: absolute;
  top: 58%;
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  text-align: center;
  
}

.raya-precio{
  position: relative;
  width: 100%;
  height: 3px;
  z-index: 1000;
  background-color: rgba(0, 0, 0, .4);
  bottom: 22px;
}



.info{
  position: absolute;
  top: 72%;
  width: 100%;
  display: flex;
  text-align: center;
  justify-content: space-evenly;
  background-color: orange;
  color: white;
}

.info div{
  width: 133px;
}

.cilindraje, .potencia{
  border-right: 0.1px solid black;
}

.btn{
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  top: 88%;
  width: 100%;
}

.btn button{
  position: absolute;
  width: 50%;
  font-family: "Poppins", sans-serif;
  font-size: 18px;
  height: 50px;
}
footer{
  display: flex;
  text-align: center;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100px;
  font-family: 'Monoton', cursive;
  background-color: black;
  color: white;
}

.regresar {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.boton {
  background-color: white;
  /* Green */
  border: none;
  color: black;
  padding: 16px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  transition-duration: 0.4s;
  cursor: pointer;
  margin-bottom: 20px;
}

.boton:hover {
  background-color: red;
  color: white;
}

</style>