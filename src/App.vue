<template>
  <div id="app">
    <form @submit.prevent="onSubmit">
      <input type="text" v-model="name">
      <button type="submit">Add</button>
    </form>
    <ul>
      <li v-for="person in people" :key="person.id">
        {{person.name}}
      </li>
    </ul>

  </div>
</template>

<script>
import axios from "axios"
const http = axios.create({
  baseURL: `http://localhost:4000`
})

export default {
  name: 'app',
  data() {
    return {
      name: "",
      people: []
    }
  },
  methods: {
    async onSubmit() {
      const person = { name: this.name }
      const response = (await http.post("people", person)).data
      this.people = [...this.people, response]
      this.name = ""
    }
  },
  async mounted() {
    this.people = (await http.get("people")).data
  }
}
</script>

<style>
* {
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  font-size: 1em;
}

#app {
  padding: 3em;
  margin: 2em;
  outline: .5em solid black;
}

ul {
  padding: 0;
}

li {
  list-style: none;
  margin: 0 10px;
}
</style>
