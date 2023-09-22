<script setup>
import axios from "axios";
import {onMounted, ref} from "vue";

const usersURL = "http://localhost:3000/users";
const users = ref([])

const newUser = {
  name: "test"
}


onMounted(async () => {
  await axios
      .get(usersURL)
      .then(res => users.value = res.data)
      .catch(err => console.error(err))
})


async function addTodo() {
  await axios
      .post(usersURL, newUser)
      .then(res => {
        users.value = [...users.value, res.data]
        newUser.name = ""
      })
      .catch(err => console.error(err))
}
</script>


<template>
  <div style="display: flex; flex-direction: column">
    <h1>Users:</h1>
    <div v-for="user in users" :key="user.id">
      {{ user.name }}
    </div>

    <input
        type="text"
        aria-label="Add Todo"
        placeholder="Add Todo"
        v-model="newUser.name"
        @keyup.enter="addTodo"
    />
  </div>

</template>


<style scoped>

</style>
