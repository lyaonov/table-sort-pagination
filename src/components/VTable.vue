<template>
    <div class="v-table">
        <div class="v-table__header">
            <p @click="sortByID">ID<i class="material-symbols-outlined"> swap_vert</i></p>
            <p @click="sortByAnswer">Answer<i class="material-symbols-outlined"> swap_vert</i></p>
            <p @click="sortByEmail">Email<i class="material-symbols-outlined"> swap_vert</i></p>
            <p>Date </p>
        </div>
        <div class="v-table__body">
            <VTableRow v-for="row in pagList" :key="row.id" :row_data="row" />
        </div>

        <div class="v-table__pagination">
            <div class="page" :class="{ 'page__selected': page === pageNumber }" @click="pageClick(page)"
                v-for="page in pages" :key="page">{{ page }}</div>
        </div>

    </div>
</template>

<script setup>
import VTableRow from './VTableRow.vue'
import { ref, onMounted, computed, watch } from 'vue';
import { useListStore, usePagListStore } from "../stores/state";

const ListStore = useListStore();
const selectList = computed(() => ListStore.list);

let usersPerPage = ref(10)
let pageNumber = ref(1)

const pagListStore = usePagListStore();
const pagList = computed(() => pagListStore.list);

const pages = computed(() => Math.ceil(selectList.value.length / 10))

function setPaginatedUsers() {
    pagListStore.clear()
    for (let i = (pageNumber.value - 1) * usersPerPage.value; i < ((pageNumber.value - 1) * usersPerPage.value) + usersPerPage.value; i++) {
        const newIt = {
            id: selectList.value[i].id,
            answer: selectList.value[i].answer,
            email: selectList.value[i].email,
            data: selectList.value[i].data
        }
        pagListStore.push(newIt)
    }
}

function pageClick(page) {
    pageNumber.value = page
    setPaginatedUsers()
}
function sortByAnswer() {
    (JSON.stringify(ListStore.list) == JSON.stringify(ListStore.list.sort((a, b) => a.answer.localeCompare(b.answer))))
        ? ListStore.list.reverse()
        : ListStore.list.sort((a, b) => a.answer.localeCompare(b.answer))
    setPaginatedUsers()
}
function sortByID() {
    (JSON.stringify(ListStore.list) == JSON.stringify(ListStore.list.sort((a, b) => a.id - b.id)))
        ? ListStore.list.reverse()
        : ListStore.list.sort((a, b) => a.id - b.id)
    setPaginatedUsers()
}
function sortByEmail() {
    (JSON.stringify(ListStore.list) == JSON.stringify(ListStore.list.sort((a, b) => a.email.localeCompare(b.email))))
        ? ListStore.list.reverse()
        : ListStore.list.sort((a, b) => a.email.localeCompare(b.email))
    setPaginatedUsers()
}
</script>



<style>
.v-table {
    width: 100%;
    margin: 0 auto;
}

.v-table__header {
    display: flex;
    justify-content: space-around;
    border: 1px solid lightgray;
    margin-bottom: 10px;
    display: flex;
}

.v-table__header p {
    flex-basis: 0%;
    display: flex;
    text-align: center;
}

.v-table__pagination {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 30px;
}

.v-table__body {
    border: 1px solid lightgray;
    border-radius: 5px;
    padding: 5px;
}

.page {
    padding: 8px;
    margin: 5px;
    border: 1px solid lightgray;
}

.page:hover {
    background: #e9e8e8;
    cursor: pointer;
}

.page__selected {
    background: #cecdcd;
}

.material-symbols-outlined {
    cursor: pointer;
    font-variation-settings:
        'FILL' 0,
        'wght' 400,
        'GRAD' 0,
        'opsz' 48
}
</style>