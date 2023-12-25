<script setup>
import { computed } from "vue";

defineEmits([
  "deleateCart",
  "incrementar",
  "decrementar",
  "agregar-Carrito",
  "vaciar-Carrito",
]);
const props = defineProps({
  recomendacion: {
    type: Object,
    required: true,
  },
  carrito: {
    type: Array,
    required: true,
  },
});
const totalPagar = computed(() => {
  return props.carrito.reduce(
    (total, element) => total + element.precio * element.cantidad,
    0
  );
});
</script>

<template>
  <header class="py-5 header">
    <div class="container-xl">
      <div class="row justify-content-center justify-content-md-between">
        <div class="col-8 col-md-3">
          <a href="index.html">
            <img class="img-fluid" src="/img/logo.svg" alt="imagen logo" />
          </a>
        </div>
        <nav
          class="col-md-6 a mt-5 d-flex align-items-start justify-content-end"
        >
          <div class="carrito">
            <img
              class="img-fluid"
              src="/img/carrito.png"
              alt="imagen carrito"
            />

            <div id="carrito" class="bg-white p-3">
              <p
                v-if="carrito.length < 1"
                class="text-center text-black my-auto"
              >
                El carrito esta vacio
              </p>
              <div v-else>
                <table class="w-100 table">
                  <thead>
                    <tr>
                      <th>Imagen</th>
                      <th>Nombre</th>
                      <th>Precio</th>
                      <th>Cantidad</th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="element in carrito" :key="element">
                      <td>
                        <img
                          class="img-fluid"
                          :src="'/img/' + element.imagen + '.jpg'"
                          alt="imagen guitarra"
                        />
                      </td>
                      <td>{{ element.nombre }}</td>
                      <td class="fw-bold">${{ element.precio }}</td>
                      <td class="flex align-items-start gap-4">
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="$emit('decrementar', element)"
                        >
                          -
                        </button>
                        {{ element.cantidad }}
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="$emit('incrementar', element)"
                        >
                          +
                        </button>
                      </td>
                      <td>
                        <button
                          class="btn btn-danger"
                          type="button"
                          @click="$emit('deleateCart', element)"
                        >
                          X
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>

                <p class="text-end">
                  Total pagar: <span class="fw-bold">${{ totalPagar }}</span>
                </p>
                <button
                  class="btn btn-dark w-100 mt-3 p-2"
                  @click="$emit('vaciar-Carrito')"
                >
                  Vaciar Carrito
                </button>
              </div>
            </div>
          </div>
        </nav>
      </div>
      <!--.row-->

      <div class="row mt-5">
        <div class="col-md-6 text-center text-md-start pt-5">
          <h1 class="display-2 fw-bold">{{ recomendacion.nombre }}</h1>
          <p class="mt-5 fs-5 text-white">
            {{ recomendacion.descripcion }}
          </p>
          <p class="text-primary fs-1 fw-black">${{ recomendacion.precio }}</p>
          <button
            type="button"
            @click="$emit('agregar-Carrito', recomendacion)"
            class="btn fs-4 bg-primary text-white py-2 px-5"
          >
            Agregar al Carrito
          </button>
        </div>
      </div>
    </div>

    <img
      class="header-guitarra"
      src="/img/header_guitarra.png"
      alt="imagen header"
    />
  </header>
</template>

<style lang="scss" scoped></style>
