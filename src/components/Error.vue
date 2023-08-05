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
        grid-column: 1 / -1;
        height: 100%;
        width: 100%;
        text-align: center;
    }
</style>