<template>
    <div class="v-table-row">
        <div class="row v-table-row-id">{{ row_data.id }}</div>
        <div class="row v-table-row-name">{{ row_data.answer }}</div>
        <div class="row v-table-row-email">{{ row_data.email }}</div>
        <div class="row v-table-row-date">{{ row_data.data }}</div>
    </div>
</template>

<script setup>
const props = defineProps({
    row_data: {
        type: Object,
        required: true
    }
})
</script>

<style>
.v-table-row {
    display: flex;
    justify-content: space-around;
}

.row {
    padding: 8px;
    flex-basis: 25%;
    text-align: center;
    border-bottom: 1px solid lightgray;
}
</style>