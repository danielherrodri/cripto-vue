<script setup>
import { ref, reactive } from 'vue';
import useCripto from './composables/useCripto';
import Alerta from './components/Alerta.vue';
import Spinner from './components/Spinner.vue';

const error = ref('');

const cotizar = reactive({
  moneda: '',
  criptomoneda: ''
});

const {
  monedas, criptoMonedas, cotizacion, cargando,
  obtenerCotizacion, mostrarResultado
} = useCripto();

const cotizarCripto = () => {
  if (Object.values(cotizar).includes('')) {
    error.value = 'Todos los campos son obligatorios.';
    return;
  }
  error.value = '';
  obtenerCotizacion(cotizar)
}

</script>

<template>
  <div class="contenedor">
    <h1 class="titulo">Cotizador de <span>Criptomonedas</span></h1>
    <div class="contenido">
      <Alerta v-if="error">
        {{ error }}
      </Alerta>
      <form @submit.prevent="cotizarCripto" class="formulario">
        <div class="campo">
          <label for="moneda">Moneda:</label>
          <select id="moneda" v-model="cotizar.moneda">
            <option value="">-- Selecciona --</option>
            <option v-for="moneda in monedas" :value="moneda.codigo">{{ moneda.texto }}</option>
          </select>
        </div>
        <div class="campo">
          <label for="cripto">Criptomoneda:</label>
          <select id="cripto" v-model="cotizar.criptomoneda">
            <option value="">-- Selecciona --</option>
            <option v-for="criptomoneda in criptoMonedas" :value="criptomoneda.CoinInfo.Name">{{
              criptomoneda.CoinInfo.FullName }}
            </option>
          </select>
        </div>
        <input type="submit" value="Cotizar" />
      </form>

      <Spinner v-if="cargando" />

      <div v-if="mostrarResultado" class="contenedor-resultado">
        <h2>Cotización</h2>
        <div class="resultado">
          <img :src="'https://cryptocompare.com/' + cotizacion.IMAGEURL" alt="imagen cripto">
          <div>
            <p>El precio es de: <span>{{ cotizacion.PRICE }}</span></p>
            <p>Precio más alto del día: <span>{{ cotizacion.HIGHDAY }}</span></p>
            <p>Precio bajo del día: <span>{{ cotizacion.LOWDAY }}</span></p>
            <p>Variación últimas 24 horas: <span>{{ cotizacion.CHANGEPCT24HOUR }}%</span></p>
            <p>Última actualización: <span>{{ cotizacion.LASTUPDATE }}</span></p>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped></style>
