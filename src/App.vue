<template>
  <div class="flex flex-col items-center w-2/12 mx-auto
  ">
    <h3 class="font-medium text-xl">Enter Github User Name</h3>
    <form @submit.prevent="newName = name" >
      <input v-model="name" class="ml-4 p-1 border-solid shadow-outline mt-4"
        placeholder="Github username" />
      <button
        class="border-red-700 mt-4 w-32 rounded-md p-2 mb-10 uppercase"
        type="submit"
      >Press me</button>
    </form>
  </div>
  <div id="list">
    <ul>
      <li v-for="lib in data" :key="lib.name">{{lib.name}} <a :href="`${lib.html_url}`">>>></a></li>

    </ul>
  </div>
</template>

<script>
import {ref, watch, reactive, toRefs} from "vue"
export default {
  name: 'App',
  components: {},
  setup() {
    const name = ref('');
    const newName = ref('');
    const state = reactive({data: []})

     watch(()=> {
      fetch(`https://api.github.com/users/${newName.value}/repos`)
      .then((response)=> response.json())
      .then((data)=> {
        state.data = data;
        console.log('data:', data)
        name.value = "";
      })
    })
    return {
      name,
      newName,
      ...toRefs(state)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#list {
  display: flex;
  justify-content: center;
  text-align: left;
}
</style>
