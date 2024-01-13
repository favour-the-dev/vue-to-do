<script setup>
  import {ref, onMounted,  watch } from 'vue';
  const todos = ref([]);
  const names = ref('')
  const input_names = ref('')
  const input_category = ref(null)
  // const todo_asc = computed(()=> todos.value.sort((a,b)=>{
  //   return b.createdAt - a.createdAt
  // }))
  watch(names, (newval)=>{
    localStorage.setItem('names', newval)
  })
  const addTodo = ()=>{
    if(input_names.value.trim() === '' || input_category.value === null){
      return
    }
    todos.value.push({
      names: input_names.value,
      category: input_category.value,
      done: false,
      createdAt: new Date().getTime()
    })
    console.log('add todo')
  }
  const removeTodo = (todo)=>{
    todos.value = todos.value.filter(t=> t != todo)
  }
  const checkCate = ()=>{
    if(input_category.value === 'buisness'){
      return true
    }
  }
  watch(todos, (newval)=>{
    localStorage.setItem('todos', JSON.stringify(newval))
  }, {deep: true})
  onMounted(()=>{
    names.value = localStorage.getItem('names') || ''
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })
  
</script>

<template>
  <div class="p-4 flex flex-col space-y-4">
    <div class="">
      <h1 class="w-full text-md md:text-xl font-bold">What's up, <input type="text" class="w-1/2 focus:outline-none" placeholder="enter name" v-model="names"></h1>
    </div>

    <div class="flex flex-col space-y-4">
      <h2>CREATE TO-DO</h2>
      <form @submit.prevent="addTodo">
        <h3 class="text-sm mb-4 font-semibold">What's on your to-do list?</h3>
        <input 
        class="p-4 border-2 border-blue-400 w-full md:w-1/2 shadow-md p-2 rounded-lg focus:outline-none" type="text" 
        placeholder="e.g Go and buy groceries" 
        v-model="input_names"
        >
        <h4 class="text-sm mb-4 font-semibold mt-4">Pick a category</h4>

        <div class="w-full md:w-1/2 flex justify-around items-center">
          <label class="flex flex-col justify-center items-center space-y-1 rounded-md shadow-md p-6 border-2 border-blue-600">
            <input 
            type="radio" 
            name="category" 
            value="buisness" 
            v-model="input_category"
            class="accent-blue-600"
            >
            <div>Buisness</div>
          </label>

          <label class="flex flex-col justify-center items-center space-y-1 rounded-md shadow-md p-6 border-2 border-pink-600">
            <input 
            type="radio" 
            name="category" 
            value="Personal" 
            v-model="input_category"
            class="accent-pink-600"
            >
            <div>Personal</div>
          </label>

        </div>
        <input type="submit" value="Add Todo" class="w-full md:w-1/2 p-4 bg-blue-400 hover:bg-blue-300 text-white font-semibold rounded-md shadow-md mt-4" >
      </form>
    </div>

    <div class="flex flex-col">
      <h2 class="mb-4">TO-DO-LIST</h2>   
      <div class="flex flex-col space-y-2">
        <div v-for="(todo, index) in todos" v-bind:class="`w-full md:w-1/2 p-4 rounded-md shadow-md flex space-x-3 items-center justify-between border-2 ${todo.done ? ' border-green-400' : 'border-red-400'}`" :key="index">
          <label>
            <input type="checkbox" :class="`${checkCate ? 'accent-blue-500' : 'accent-pink-500'}`" v-model="todo.done">
          </label>
          <div>
            <input type="text" class="text-center capitalize" v-model="todo.names">
          </div>
          <button class="p-1 rounded-md bg-red-500 text-white text-sm font-semibold" @click="removeTodo(todo)">Delete</button>
        </div>
      </div>   
    </div>
  </div>
</template>



