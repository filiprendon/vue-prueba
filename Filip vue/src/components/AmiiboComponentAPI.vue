<template>
  <div>
    <h2>{{ title }}</h2>
    <div>
      <button @click="searchType = 'serie'">Buscar por serie</button>
      <button @click="searchType = 'personaje'">Buscar por personaje</button>
    </div>
    <div v-if="searchType === 'serie'">
      <select v-model="selectedSerie">
        <option v-for="serie in series" :key="serie" :value="serie">{{ serie }}</option>
      </select>
      <button @click="fetchData(selectedSerie)">Buscar</button>
    </div>
    <div v-else-if="searchType === 'personaje'">
      <select v-model="selectedCharacter">
        <option v-for="amiibo in amiibos" :key="amiibo.character" :value="amiibo.character">{{ amiibo.character }}</option>
      </select>
      <button @click="fetchSingleAmiibo(selectedCharacter)">Buscar</button>
    </div>
    <div v-if="!amiibos">Loading...</div>
    <div v-else>
      <div v-for="amiibo in amiibos" :key="amiibo.head">
        <h3>{{ amiibo.character }}</h3>
        <img :src="amiibo.image">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "Amiibos",
      amiibos: null,
      series: ['Fire Emblem', 'Animal Crossing', 'Mario Sports Superstars'],
      selectedSerie: '',
      selectedCharacter: '',
      searchType: 'serie'
    };
  },
  methods: {
    fetchSingleAmiibo(character) {
      fetch(`https://www.amiiboapi.com/api/amiibo/?name=${character}`)
        .then(response => response.json())
        .then(data => this.amiibos = data.amiibo)
        .catch(err => console.log("Error fetching API: " + err.message));
    },
    fetchData(serie) {
      fetch(`https://www.amiiboapi.com/api/amiibo/?amiiboSeries=${serie}`)
        .then(response => response.json())
        .then(data => this.amiibos = data.amiibo)
        .catch(err => console.log("Error fetching API: " + err.message));
    }
  }
};
</script>
