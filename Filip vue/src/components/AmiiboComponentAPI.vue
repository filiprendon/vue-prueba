<template> 
    <h2>{{ this.title }} </h2>
    <div v-if="!amiibos">Loading...</div>
    <div v-else>
        <div v-for="amiibo in this.amiibos" :key="amiibo.head">
            <h3> {{ amiibo.character }}</h3>
            <img :src="amiibo.image">
        </div>
    </div>

  </template>
<script>
  export default {
    data() {
      return {
        title: "Fire Emblem amiibos",
        amiibos: null,
      };
    },
    methods: {  
    fetchData() {
        fetch('https://www.amiiboapi.com/api/amiibo/?amiiboSeries=Fire%20Emblem')
            .then(response => response.json())
            .then(data => this.amiibos = data.amiibo)
            .catch(err => console.log("Error fetching API: " + err.message));   
    }
    // Alternativa
    //    async fetchData() {      
    //     const response = await fetch("https://www.amiiboapi.com/api/amiibo/?amiiboSeries=Fire%20Emblem");      
    //     const data = await response.json();
    //     this.amiibos = data.amiibo;
    //   }
    },
    mounted() {
      this.fetchData();
    },
  };
</script>