<script setup>
import { computed } from 'vue'

const props = defineProps({
  character: Object,
  color: Number
});

const heightInMetres = computed(() => {
    const height = new Intl.NumberFormat ("en-US", {
        style: "unit",
        unit: "meter"
    }).format(parseInt(props.character.height)/100);
    return height;
});

const weightInKg = computed(() => {
    const weight = new Intl.NumberFormat ("en-US", {
        style: "unit",
        unit: "kilogram"
    }).format(parseInt(props.character.mass));

    return weight;
});

const createdDate = computed(() => {
    const date = new Intl.DateTimeFormat('en-US', { day: "2-digit", month: "2-digit", year: "numeric"}).format(new Date(props.character.created));
    return date;
});

</script>
<template>
    <div class="card">
        <h1>{{ character.name }}</h1>
        <div class="details">
            <p><span>Height:</span> {{ heightInMetres }}</p>
            <p><span>Mass:</span> {{ weightInKg }}</p>
            <p><span># of Films:</span> {{ character.films.length }}</p>
            <p><span>Birth Date:</span> {{ character.birth_year }}</p>
            <p><span>Date Added:</span> {{ createdDate }}</p>
        </div>
    </div>
</template>
<style scoped>
    .card {
        background-color: hsl(v-bind(color) 100% 25%);
        border-radius: 8px;
        height: 14rem;
        overflow: hidden;
        padding: .5rem;
        width: 9.375rem;
    }

    .details {
        display: none;
        margin: 2rem 0 0;
    }

    .card:hover .details{
        display: block;
    }

    h1 {
        font-size: 1.2rem;
        line-height: 1.1;
        margin: 0;
    }

    p {
        margin: 0;
    }
</style>