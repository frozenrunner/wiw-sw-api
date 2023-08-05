<script setup>
import { onMounted, ref } from 'vue'
import Character from './components/Character.vue'
import Error from './components/Error.vue'

const errorState = ref(false);
const characters = ref([]);
const colours = ref([]);

//Get data, keep getting until there is no more to get
async function getData(dataArray, url) {
  let completed = false;
  do {
    try {
      var response = await fetch(url);
      var page = await response.json();

      if (dataArray.value.length === 0) { //init the collection to set the loading state
        dataArray.value = Array(page.count).fill({
          name: "Loading",
          height: 0,
          mass: 0,
          created: "1900-01-01",
          films: [],
          birth_year: ""
        });
      }

      //swap out the loading cards for character cards
      const firstNullIndex = dataArray.value.findIndex((e) => e.name === "Loading");
      dataArray.value.splice(firstNullIndex, page.results.length, ...page.results);

      if (page.next !== null) {
        url = page.next;
      }

      completed = page.next === null;
    } catch (error) {
      console.error(error.message);
      completed = true;
      errorState.value = true;
    }

  } while (completed === false);
}

//Get the record count and use the callback
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

//Get the species colour for the given species index
function getSpeciesColor(character) {
  if (character === null || character.species === undefined || character.species === null) {
    return colours.value[0];
  }

  const species = character.species;

  if (species.length === 0) {
    return colours.value[0];
  }

  const currentSpecies = species[0];
  const speciesColour = colours.value[species[0].substring(currentSpecies.length - 2, currentSpecies.length -1)];
  return speciesColour;
}

onMounted(async () => {
    Promise.all([getData(characters, "https://swapi.dev/api/people/"), getCount("https://swapi.dev/api/species/", generateColours)]);
});
</script>

<template>
  <Error v-show="errorState" :message="'Error retrieving data'"/>
  <Character v-show="errorState === false" v-for="character in characters" :character="character" :color="getSpeciesColor(character)"/>
</template>