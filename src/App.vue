<script setup>
import axios from 'axios';
import jsonpAdapter from 'axios-jsonp';
import { ref, onMounted, computed, watch } from 'vue';
import VTable from './components/VTable.vue'

import { useListStore, usePagListStore } from "./stores/state";

const ListStore = useListStore();
const selectList = computed(() => ListStore.list);

const pagListStore = usePagListStore();
const pagList = computed(() => pagListStore.list);



const table = ref([])
onMounted(() => {
  axios({
    url: 'https://jsonplaceholder.typicode.com/comments',
    adapter: jsonpAdapter,
    method: "GET",
  }).then(({ data }) => data.forEach((item) => {
    const newItem = {
      id: item.id,
      answer: `${item.name.split(' ')[0]} ${item.name.split(' ')[1]} ${item.name.split(' ')[2]}`,
      email: item.email,
      data: `${Math.floor(Math.random() * 100)}.${Math.floor(Math.random() * 10)}.2022`
    }
    ListStore.push(newItem)
  })).then(() => {
    pagListStore.clear()
    for (let i = 0; i < 10; i++) {
      selectList.value
      const newIt = {
        id: selectList.value[i].id,
        answer: selectList.value[i].answer,
        email: selectList.value[i].email,
        data: selectList.value[i].data
      }
      pagListStore.push(newIt)
    }
  })

})
</script>

<template>
  <header>
    <h1>Таблица</h1>
  </header>
  <hr>
  <VTable :data='table' />
</template>

<style scoped>
header {
  text-align: center;
  border: 3px solid lightgray;
  border-radius: 5px;
}
</style>
