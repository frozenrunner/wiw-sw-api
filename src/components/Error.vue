<script setup>
const props = defineProps({
  message: String
});
</script>

<template>
    <div class="error">
        <h1>Error</h1>
        <p>{{ message }}</p>
    </div>
</template>

<style scoped>
    .error {
        display: fixed;
        grid-column: 1 / -1;
        height: 20rem;
        width: 20rem;
    }
</style>