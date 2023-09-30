<script setup>
import axios from "axios";
import {onMounted, ref} from "vue";

const usersURL = "http://localhost:3000/users";
const users = ref([])

const newUser = {
  name: "test_0",
  details: "test_0_details",
  avatar: "0.jpg",
  login: "log_0",
  password: "p_0",
}

const activeUser = {
  name: "test_0",
  details: "test_0_details",
  avatar: require(`./assets/images/"0.jpg"`),
}


onMounted(async () => {
  await axios
      .get(usersURL)
      .then(res => users.value = res.data)
      .catch(err => console.error(err))
})


async function addUser() {
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
  <header>
    <div class="container">
      <div class="d-flex justify-content-end">
        <div class="d-flex align-items-center gap-2">
          <h6 class="m-0">
            {{ activeUser.name }}
          </h6>
          <img class="rounded-circle border"
               :src="activeUser.avatar"
               alt=""
               width="35" height="35">
        </div>
      </div>
    </div>
  </header>

  <div class="container d-flex flex-column py-5">
    <div class="d-flex flex-column mb-5">
      <h1>Users:</h1>

      <div class="d-flex gap-2">
        <input
            class="form-control"
            type="text"
            placeholder="Add name"
            v-model="newUser.name"
        />
        <input
            class="form-control"
            type="text"
            placeholder="Add details"
            v-model="newUser.details"
        />
        <input
            class="form-control"
            type="text"
            placeholder="Add name"
            v-model="newUser.login"
        />
        <input
            class="form-control"
            type="text"
            placeholder="Add details"
            v-model="newUser.password"
        />
        <button class="btn btn-primary px-5" @click="addUser">
          Add
        </button>
      </div>
    </div>


    <div class="row g-3 align-items-stretch">
      <div v-for="user in users"
           :key="user.id"
           class="col col-lg-4 col-xl-3">
        <div class="border rounded d-flex flex-column align-items-center text-center h-100 pt-5 p-2">
          <img class="rounded-circle border"
               src="#"
               alt=""
               width="150" height="150">
          <h2 class="fw-normal">
            {{ user.name }}
          </h2>
          <p class="flex-grow-1">
            {{ user.details }}
          </p>
          <p><a class="btn btn-primary" href="#">
            Add Friend &raquo;
          </a></p>
        </div>
      </div>

    </div>
  </div>
</template>


<style scoped>

</style>
