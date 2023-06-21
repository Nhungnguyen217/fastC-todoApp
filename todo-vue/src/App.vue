<script setup>
import { ref, onMounted, computed, watch } from "vue"
const todos = ref([]) //create empty arr
const name = ref("") //create empty string
const input_content = ref("")
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
  localStorage.setItem("name", newVal)
})


</script>
  
<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">Hi, this is to-do list of <input type="text" placeholder="name here" v-model="name" /></h2>
    </section>
  </main>
</template>


