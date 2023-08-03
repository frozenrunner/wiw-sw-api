<script setup>
import { onMounted, ref } from 'vue'
import Card from './components/Card.vue'

const characters = ref([]);
const colours = ref([]);

onMounted(async () => {
    Promise.all([getData(characters, "https://swapi.dev/api/people/"), getCount("https://swapi.dev/api/species/", generateColours)]);
});

async function getData(dataArray, url) {
  let completed = false;
  do {
    var response = await fetch(url);
    var page = await response.json();

    dataArray.value = dataArray.value.concat(page.results);
    console.log(dataArray.value);

    if (page.next !== null) {
      url = page.next;
    }
    completed = page.next === null;

  } while (completed === false);
}

async function getCount(url, callback) {
  var response = await fetch(url);
  var data = await response.json();

  if (data.count !== undefined && callback !== undefined) {
    callback(data.count);
  }
}

//Generate colour values based on the number of species to at least attempt some kind of separation
function generateColours(total) {
    var i = 360 / (total - 1); // distribute the colors evenly on the hue range
    for (var x=0; x<total; x++)
    {
      colours.value.push(i * x);
    }
}

function getSpeciesColor(species) {
  if (species.length === 0) {
    return colours.value[0];
  }

  const currentSpecies = species[0];
  const speciesColour = colours.value[species[0].substring(currentSpecies.length - 2, currentSpecies.length -1)];
  return speciesColour;
}

</script>

<template>
  <Card v-for="character in characters" :character="character" :color="getSpeciesColor(character.species)"/>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>