<script setup>
import { ref, onMounted, computed, watch } from "vue"

// tạo các biến, mảng dữ liệu
const todos = ref([]) //create empty arr
const name = ref("") //create empty string
const input_content = ref("")
const input_category = ref(null)

// các hàm thực thi
const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

//thêm todo vào danh sạc to-do list
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

  console.log(input_content.value)
  input_content.value = ''
  input_category.value = null
}

// hàm xóa todo
const removeTodo = todo => { todos.value = todos.value.filter(t => t !== todo) }

//lưu vào localStorage
watch(todos, newVal => {
  localStorage.setItem("todos", JSON.stringify(newVal))
}, { deep: true })

watch(name, newVal => {
  localStorage.setItem("name", newVal)
})

onMounted(() => {
  name.value = localStorage.getItem("name") || ""
  todos.value = JSON.parse(localStorage.getItem("todos")) || []
})
</script>
  
<!-- Sắp xếp -->
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

        <!-- TinyMCE format -->
        <textarea placeholder="e.g Make a to-do app by Vue3" v-model="input_content"></textarea>


        <!-- Thiết lập thời hạn cho to-do -->
        <h4 id="duration">Set duration:</h4>

        <!-- Phân loại to-do -->
        <h4>Pick a category</h4>
        <div class=" options">
          <label> <input type="radio" name="category" id="category1" value="business" v-model="input_category" /><span
              class="bubble business"></span>
            <div>Work</div>
          </label>
          <label> <input type="radio" name="category" id="category2" value="personal" v-model="input_category" /><span
              class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add todo" />
      </form>
    </section>

    <!-- Hiển thị danh sách to-do -->
    <section class="todo-list">
      <h3>TO-DO LIST</h3>
      <div class="list">
        <!-- lấy từ todos_asc -->
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done">
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>
          <div class="action">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>


