<template>
  <div>
    <input type="text" v-model="searchTerm" placeholder="Search" />
    <table class="my-table">
      <thead>
        <tr>
          <th>Official name</th>
          <th>Capital</th>
          <th>Subregion</th>
          <th>Population</th>
          <th>Languages</th>
          <th>Flag</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="country in filteredCountries" :key="country.name.common">
          <td>{{ country.name.official }}</td>
          <td>{{ getCapital(country.capital) }}</td>
          <td>{{ country.subregion }}</td>
          <td>{{ country.population }}</td>
          <td>{{ getLanguageNames(country.languages) }}</td>
          <td>
            <img :src="country.flags.png" alt="Flag" style="max-width: 40px;" />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const urlApi = 'https://restcountries.com/v3.1/region/europe';

const countries = ref([]);
const searchTerm = ref("");

const recuperaCountries = async () => {
  try {
    const response = await fetch(urlApi);
    const data = await response.json();
    countries.value = data;
  } catch (error) {
    console.error(error);
  }
}

recuperaCountries();

const filteredCountries = computed(() => {
  if (searchTerm.value === "") {
    return countries.value;
  }
  const term = searchTerm.value.toLowerCase();
  return countries.value.filter(country =>
    country.name.official.toLowerCase().includes(term) ||
    getCapital(country.capital).toLowerCase().includes(term) ||
    country.subregion.toLowerCase().includes(term) ||
    country.population.toString().includes(term) ||
    getLanguageNames(country.languages).toLowerCase().includes(term)
  );
});

const getCapital = (capital) => {
  if (Array.isArray(capital)) {
    return capital.join(", ");
  } else {
    return capital;
  }
}

function getLanguageNames(languages) {
  if (Array.isArray(languages)) {
    return languages.join(", ");
  } else if (typeof languages === "object") {
    return Object.values(languages).join(", ");
  } else {
    return languages;
  }
}
</script>
