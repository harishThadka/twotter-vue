<template>
  <div class="home">
    <h1>Home page</h1>

    <router-link v-for="user in users" :to="{name: 'UserProfile',params: {userId: user.id} }" :key="user.id">

      <li>
              {{ user.username}}
      </li>

    </router-link>
  </div>
</template>

<script>
// @ is an alias to /src

import {users} from '../assets/users.js'

export default {
  name: 'Home',
  data(){
    return {
        users
    }

  }
}
</script>
