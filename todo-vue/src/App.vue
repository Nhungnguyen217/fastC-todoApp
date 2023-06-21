<script setup>
import { ref, onMounted, computed, watch } from "vue"
import { DatePickerComponent as EjsDatepicker } from '@syncfusion/ej2-vue-calendars';
import { TimePickerComponent as EjsTimepicker } from "@syncfusion/ej2-vue-calendars";

var today = new Date()
const todos = ref([])
const name = ref("")
const input_content = ref("")
const input_category = ref(null)
const input_date = ref(new Date())
const input_time = ref(new Date())

console.log("Thời gian hiện tại: " + today)
console.log(typeof (todos))

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
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
    deadline: input_date.value.getTime(),
    dateEnd: input_date.value.getDate() + "-" + input_date.value.getMonth() + "-" + input_date.value.getFullYear() + " at " + input_time.value.getHours() + ":" + input_time.value.getMinutes(),
  })

  console.log(input_content.value)
  input_content.value = ''
  input_category.value = null
}

const removeTodo = todo => { todos.value = todos.value.filter(t => t !== todo) }

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

        <h4 id="duration">Set duration:</h4>
        <div class="control_wrapper">
          <ejs-datepicker v-model="input_date"></ejs-datepicker>
          <ejs-timepicker v-model="input_time"></ejs-timepicker>
        </div>

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

    <section class="todo-list">
      <h3>TO-DO LIST</h3>
      <div class="list">
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done">
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <!-- timeEnd -->
          <div class="todo-dateEnd">
            <input type="text" v-model="todo.dateEnd" />
          </div>

          <div class="action">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>

    <!-- Alert -->
    <div>
      <div class="toast" role="alert" aria-live="assertive" aria-atomic="true">
        <div class="toast-header">
          <img src="..." class="rounded mr-2" alt="...">
          <strong class="mr-auto">Bootstrap</strong>
          <small>11 mins ago</small>
          <button type="button" class="ml-2 mb-1 close" data-dismiss="toast" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="toast-body">
          Oops, you miss to-do!
        </div>
      </div>
    </div>

  </main>
</template>


