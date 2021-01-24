<template>
  <div class="user-profile">
    <router-link to="/">
    <div>
      Home
    </div>
    </router-link>

    @{{ user.username }} {{ fullName }}
    <br />
    {{ computeUserId }}
    <strong>Followers</strong>
    {{ followers }}
    <button @click="followUser">Follow</button>
    <div class="admin" v-if="user.isAdmin">Admin</div>
    <div class="admin" v-else>Not Admin</div>

    <form @submit.prevent="createTwoot">
      <label for="newTwoot">Enter New Twoot ({{ newTwootLen }}/20)</label><br />
      <textarea
        id="newTwoot"
        class="newTwoot"
        rows="4"
        v-model="newTwoot"
        :class="{ '--excedded': newTwootLen > 20 }"
      ></textarea
      ><br />
      <select id="newTwootType" v-model="selectType">
        <option
          :value="type.value"
          v-for="(type, index) in twootTypes"
          :key="index"
        >
          {{ type.value }}
        </option>
      </select>

      <button>Twoot</button>
    </form>
    <!-- 
    <div v-for="twoot in user.twoots" :key="twoot.id" >
      <ul>
        <li>{{ twoot.content }}</li>
      </ul>
    </div> -->

    <twoot-item
      v-for="twoot in user.twoots"
      :key="twoot.id"
      :username="user.username"
      :twoot="twoot"
      @favourite="toggleFavourite"
    >
    </twoot-item>
  </div>
</template>

<script>
import TwootItem from "../components/TwootItem";
import { useRoute } from "vue-router";

import {users} from "../assets/users.js";

export default {
  name: "UserProfile",
  components: {
    "twoot-item": TwootItem,
  },
  data() {
    return {
      route: useRoute(),
      userId: 5,
      newTwoot: "",
      selectType: "instant-post",
      twootTypes: [
        { name: "Draft", value: "draft" },
        { name: "instant", value: "instant-post" },
      ],
      followers: 0,
      user: users[this.computeUserId- 1] || users[1],
    };
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
    newTwootLen() {
      return this.newTwoot.length;
    },
    computeUserId() {
      console.log(`this is route ${this.route.params.userId}`)

      return this.route.params.userId;
    }
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavourite(id) {
      console.log(`this is favourtie twoot ${id}`);
    },
    createTwoot() {
      if (this.newTwoot != "" && this.selectType != "draft") {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwoot,
        });
        this.newTwoot = "";
      }
    },
    funcUserId() {
            console.log(`this is route ${this.route.params.userId}`)

      this.userId = this.route.params.userId;
    }

  },
  watch: {
    followers(newCount, oldCount) {
      if (oldCount < newCount) {
        console.log("Hey! you gained follower");
      }
    },
  },
  mounted() {
    this.followUser();
  },
};
</script>

<style lang="scss" scoped>
.user-profile {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}
.admin {
  background-color: black;
  color: white;
  width: 100px;
  text-align: center;
  border-radius: 2px;
}
#newTwootType {
  width: 100px;
}

.newTwoot {
  border: 2px solid green;

  &.--excedded {
    border: 5px solid red;
  }
}
</style>
