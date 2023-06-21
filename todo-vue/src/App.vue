<script setup>
import { ref, onMounted, computed, watch } from "vue"
const todos = ref([]) //create empty arr
const name = ref("") //create empty string
const input_content = ref("")
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))

const addTodo = () => {
  if (input_content.value.trim() === "" | input_category.value === null) {
    return
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),

  })
}

// localStorage
watch(todos, newVal => {
  localStorage.setItem("todos", JSON.stringify(newVal))
}, { deep: true })

watch(name, newVal => {
  localStorage.setItem("name", newVal)
})

onMounted(() => {
  name.value = localStorage.getItem("name") || ""
})
</script>
  
<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">Hi, this is to-do list of <input type="text" placeholder="name here" v-model="name" /></h2>
    </section>
    <section class="create-todo">
      <h3>CREATE A TO-DO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your to-do list?</h4>
        <input type="text" placeholder="e.g Make a to-do app by Vue3 " v-model="input_content" />

        <!-- Set timeEnd of duration-->
        <h4>Set duration:</h4>

        <h4>Pick a category</h4>
        <div class="options">
          <label> <input type="radio" name="category" id="category1" value="Work" v-model="input_category" /><span
              class="bubble business"></span>
            <div>Work</div>
          </label>

          <label> <input type="radio" name="category" id="category2" value="Personal" v-model="input_category" /><span
              class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add todo" />
      </form>
    </section>
  </main>
</template>


