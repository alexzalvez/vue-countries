<script setup>
// Importamos la función 'ref' de Vue
import { ref } from 'vue';

// Definimos la URL de la API
const urlApi = 'https://restcountries.com/v3.1/region/europe';

// Creamos una referencia reactiva llamada 'countries'
const countries = ref([]);

// Definimos la función 'recuperaCountries'
function recuperaCountries() {
  // Hacemos una solicitud fetch a la URL de la API
  fetch(urlApi)
    // Parseamos la respuesta a formato JSON
    .then((respuesta) => respuesta.json())
    // Manipulamos los datos obtenidos
    .then((data) => {
      // Imprimimos los datos en la consola
      console.log(data);
      // Asignamos los datos a la referencia reactiva 'countries'
      countries.value = data;
    });
}

// Llamamos a la función 'recuperaCountries' para obtener los datos
recuperaCountries();
</script>

<template>
  <!-- Creamos una tabla con la clase 'my-table' -->
  <table class="my-table">
    <thead>
      <tr>
        <th>Official name</th>
        <th>Capital</th>
        <th>Subregion</th>
        <th>Population</th>
        <th>Languages</th>
      </tr>
    </thead>
    <tbody>
      <!-- Iteramos sobre los países en la referencia reactiva 'countries' -->
      <tr v-for="country in countries" :key="country.name.common">
        <!-- Mostramos el nombre oficial del país -->
        <td>{{ country.name.official }}</td>
        <!-- Mostramos la capital del país, separando los valores si es un array -->
        <td>{{ Array.isArray(country.capital) ? country.capital.join(", ") : country.capital }}</td>
        <!-- Mostramos la subregión del país -->
        <td>{{ country.subregion }}</td>
        <!-- Mostramos la población del país -->
        <td>{{ country.population }}</td>
        <!-- Mostramos los nombres de los idiomas del país, separando los valores si es un array o un objeto -->
        <td>{{ getLanguageNames(country.languages) }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
// Definimos la función 'getLanguageNames' para obtener los nombres de los idiomas
function getLanguageNames(languages) {
  // Verificamos si 'languages' es un array
  if (Array.isArray(languages)) {
    // Si es un array, unimos los elementos con una coma como separador
    return languages.join(", ");
  } else if (typeof languages === "object") {
    // Si es un objeto, obtenemos los valores y los unimos con una coma como separador
    return Object.values(languages).join(", ");
  } else {
    // En cualquier otro caso, devolvemos 'languages' sin modificar
    return languages;
  }
}
</script>
