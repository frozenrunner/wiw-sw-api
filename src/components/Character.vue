<script setup>
import { computed, ref } from 'vue'
import HomeworldModal from './HomeworldModal.vue'

const props = defineProps({
  character: Object,
  color: Number,
});

const modalOpen = ref(false);

const loading = computed(() => {
    return props.character.name === "Loading";
});

const heightInMetres = computed(() => {
    if (props.character.height === undefined || props.character.height === null) {
        return 0;
    }
    
    const height = new Intl.NumberFormat ("en-US", {
        style: "unit",
        unit: "meter"
    }).format(parseInt(props.character.height)/100);
    return height;
});

const weightInKg = computed(() => {
    if (props.character.mass === undefined || props.character.mass === null) {
        return 0;
    }
    const weight = new Intl.NumberFormat ("en-US", {
        style: "unit",
        unit: "kilogram"
    }).format(parseInt(props.character.mass));

    return weight;
});

const createdDate = computed(() => {
    if (props.character.created === undefined || props.character.created === null) {
        return 0;
    }
    const date = new Intl.DateTimeFormat('en-US', { day: "2-digit", month: "2-digit", year: "numeric"}).format(new Date(props.character.created));
    return date;
});

function openModal() {
    modalOpen.value = true;
}

function closeModal() {
    modalOpen.value = false;
}
</script>

<template>
    <div class="card character-card" :class="{'is-loading': loading}" @click="openModal">
        <div class="border-one"></div>
        <div class="border-two"></div>
        <h1>{{ character.name }}</h1>
        <div class="details">
            <p><span>Height:</span> {{ heightInMetres }}</p>
            <p><span>Mass:</span> {{ weightInKg }}</p>
            <p><span># of Films:</span> {{ character.films.length }}</p>
            <p><span>Birth Date:</span> {{ character.birth_year }}</p>
            <p><span>Date Added:</span> {{ createdDate }}</p>
        </div>
    </div>
    <HomeworldModal v-if="modalOpen" :open="modalOpen" @close="closeModal" :homeworld="character.homeworld" :color="color"/>
</template>

<style>
    .card {
        background-color: #000;
        border-radius: 8px;
        height: 16rem;
        overflow: hidden;
        padding: 1.5rem;
        position: relative;
        transform: scale(1,1);
        width: 14rem;
    }

    .card.characger-card:hover {
        box-shadow: 0 10px 24px hsla(0, 0%, 0%, .2);
        transform: scale(1.1, 1.1);
        transition: transform 150ms ease-in;
    }

    .border-one, .border-two {
        border: .4375rem solid hsl(v-bind(color) 100% 25%);
        position: absolute;
    }

    .border-one {
        border-radius: 1.5rem;
        height: calc(100% - 1.875rem);
        margin: -1rem 0 0 -1rem;
        width: calc(100% - 1.875rem);
    }

    .border-two {
        border-radius: 14px;
        height: calc(100% - 3rem);
        margin: -.4375rem 0 0 -.4375rem;
        width: calc(100% - 3rem);
    }
    
    .card.is-loading .border-one, .card.is-loading .border-two {
        border: 7px solid grey;
    }

    .details {
        margin: 2rem 0 0;
        padding: 0 .5rem;
    }

    .card.character-card .details {
        display: none;
    }

    .card.character-card:hover:not(.is-loading) .details{
        display: block;
    }

    .details.is-loading {
        display: none;
    }

    h1 {
        font-size: 1.2rem;
        line-height: 1.1;
        margin: 0;
    }

    p {
        margin: 0;
    }

    p span:first-child {
        font-weight: 700;
    }
</style>