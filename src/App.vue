<script setup>
import { ref, reactive, onMounted, watch } from "vue";
import { db } from "./data/guitarras.js";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";

const guitarras = ref([]);
const carrito = ref([]);
const recomendacion = ref(db[2]);

watch(
  carrito,
  () => {
    localStorage.setItem("carrito", JSON.stringify(carrito.value));
  },
  { deep: true }
);

function addCart(elementosCart) {
  const existe = carrito.value.findIndex(
    (element) => element.id === elementosCart.id
  );
  if (existe === -1) {
    elementosCart.cantidad = 1;
    carrito.value.push(elementosCart);
  } else {
    carrito.value[existe].cantidad++;
  }
}
function deleateCart(elementosCart) {
  carrito.value = carrito.value.filter(
    (element) => element.id !== elementosCart.id
  );
}
function incrementar(elementoCar) {
  elementoCar.cantidad++;
}

function decrementar(elementoCar) {
  if (elementoCar.cantidad > 1) {
    elementoCar.cantidad--;
  }
}

function vaciarCarrito() {
  carrito.value = [];
}

onMounted(() => {
  guitarras.value = db;
  if (localStorage.getItem("carrito")) {
    carrito.value = JSON.parse(localStorage.getItem("carrito"));
  }
});
</script>
<template>
  <Header
    @deleateCart="deleateCart"
    @incrementar="incrementar"
    @decrementar="decrementar"
    @agregar-Carrito="addCart"
    @vaciar-Carrito="vaciarCarrito"
    :recomendacion="recomendacion"
    :carrito="carrito"
  ></Header>
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="element in guitarras"
        :key="element"
        :guitarra="element"
        @agregar-Carrito="addCart"
      ></Guitarra>
    </div>
  </main>

  <footer class="bg-dark mt-5 py-5">
    <div class="container-xl">
      <p class="text-white text-center fs-4 mt-4 m-md-0">
        GuitarLA - Todos los derechos Reservados
      </p>
    </div>
  </footer>
</template>

<style lang="scss" scoped></style>
