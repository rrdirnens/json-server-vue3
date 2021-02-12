<template>
  <HelloWorld msg="JSON server + Vue 3" />
  <h1>Users:</h1>
  <div class="form-wrap">
    <label for="new-user">Enter new user name: </label>
    <input type="text" name="new-user" required v-model="userName" />
    <label for="new-user-streak">Enter new user streak: </label>
    <input
      type="text"
      name="new-user-streak"
      required="true"
      v-model="userStreak"
    />
    <button class="submit" @click="addUser">Add new user</button>
  </div>

  <ul>
    <li v-for="user of users" :key="user.id">
      {{ user.name }} has a streak of {{ user.streakInDays }}
    </li>
  </ul>
</template>

<script>
import axios from "axios";
import HelloWorld from "./components/HelloWorld.vue";

const baseURL = "http://localhost:3000/users";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      users: [],
      userName: "",
      userStreak: null,
    };
  },
  async created() {
    try {
      const res = await axios.get(baseURL);

      this.users = res.data;
    } catch (e) {
      console.error(e);
    }
  },
  methods: {
    async addUser() {
      const res = await axios.post(baseURL, {
        name: this.userName,
        streakInDays: this.userStreak,
      });
      console.log(res.data);
      this.users = [...this.users, res.data];
      this.userName = "";
      this.userStreak = "";
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.form-wrap {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  height: 200px;
  width: 100px;
}

ul {
  list-style: none;
  padding: 0;
  margin-top: 40px;

  li {
    margin-bottom: 10px;
    font-weight: 600;
  }
}

a {
  color: #42b983;
}
</style>
