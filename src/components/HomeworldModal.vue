<script setup>
import { computed, onMounted, ref } from 'vue'
import Error from "./Error.vue"
const props = defineProps({
    homeworld: String,
    open: {
        type: Boolean,
        default: false
    }
});

defineEmits(['close']);

const errorState = ref(false);
const homeworldData = ref({
    name: "Loading",
    terrain: "",
    climate: "",
    residents: []
});

const loading = computed(() => {
    return homeworldData.value.name === "Loading";
});

async function getHomeworld(url) {
    if (url !== null && url !== undefined) {
        try {
            const response = await fetch(url);
            homeworldData.value = await response.json();
        } catch (error) {
            console.error(error.message);
            errorState.value = true;
        }
    }
}

const numberOfResidents = computed(() => {
    return homeworldData.value.residents.length;
});

onMounted(async () => await getHomeworld(props.homeworld));
</script>

<template>
    <div class="modal" :class="{'is-active': open}">
        <div class="modal-background" @click="$emit('close')"></div>
        <div class="modal-content card">
            <div v-show="errorState === false">
                <div class="border-one"></div>
                <div class="border-two"></div>
                <h1>{{ homeworldData.name }}</h1>
                <div class="details" :class="{'is-loading': loading }">
                    <p><span>Terrain:</span> <span class="homeworld-span">{{ homeworldData.terrain }}</span></p>
                    <p><span>Climate:</span> <span class="homeworld-span">{{ homeworldData.climate }}</span></p>
                    <p><span>Number of Residents:</span> <span class="homeworld-span">{{ numberOfResidents }}</span></p>
                </div>
            </div>
            <Error v-show="errorState" :message="'Error retrieving data'"/>
        </div>
        <button class="modal-close is-large" aria-label="close" @click="$emit('close')"></button>
    </div>
</template>

<style scoped>
    .card {
        background-color: #fff;
        color: #000;
    }

    .details .homeworld-span {
        text-transform: capitalize;
    }

    .error {
        height: 100%;
        width: 100%;
    }
</style>