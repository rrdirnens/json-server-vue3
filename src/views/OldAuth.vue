<template>
  <h1>Home (OldAuth.vue)</h1>

  <div class="register-user">
    <div class="title">Register, please:</div>
    <input
      type="text"
      class="user-name"
      v-model="registration.user"
      placeholder="Username"
    />
    <input
      type="password"
      class="user-password"
      v-model="registration.pw"
      placeholder="Password"
    />
    <button class="reg-submit" @click="registerUser">Register</button>
  </div>

  <div class="login-user" style="margin-top: 20px">
    <div class="title">Log in, please:</div>
    <input
      type="text"
      class="user-name"
      v-model="login.user"
      placeholder="Username"
    />
    <input
      type="password"
      class="user-password"
      v-model="login.pw"
      placeholder="Password"
    />
    <button class="reg-submit" @click="loginUser">Login</button>
  </div>

  <button class="check-logged-in" @click="checkLoggedIn">
      check if auth token is present
  </button>

    <button class="check-db-connection" @click="getTodos">
        get protected data from db
    </button>

     <button class="check-db-connection" @click="logOut">
        revoke access / log out current user
    </button>
  <!-- <div style="">
    <h1>Todos:</h1>
    <div class="form-wrap">
      <label for="new-user">Enter new todo: </label>
      <input type="text" name="new-user" v-model="todoName" />
      <button class="submit" @click="addTodo">Submit</button>
    </div>
  </div>-->

  <ul style="">
    <li v-for="todo of todos" :key="todo.id">
      {{ todo.todo }}
    </li>
  </ul> 
</template>

<script>
import axios from "axios";
// import HelloWorld from "./components/HelloWorld.vue";

const baseURL = "http://localhost:3000/todos";
const loginURL = "http://localhost:3000/auth/login";
const registerURL = "http://localhost:3000/auth/register";

export default {
  name: "App",
  components: {
    // HelloWorld,
  },
  data() {
    return {
      todos: [],
      todoName: "",
      accessToken: "",
      registration: {
        user: "",
        pw: "",
      },
      login: {
        user: "",
        pw: "",
      },
    };
  },
  async created() {},
  methods: {
    async addTodo() {
      const res = await axios.post(baseURL, {
        todo: this.todoName,
      });
      console.log(res.data);
      this.todos = [...this.todos, res.data];
      this.todoName = "";
    },

    async registerUser() {
      try {
        const res = await axios({
          method: "post",
          url: registerURL,
          data: {
            email: this.registration.user,
            password: this.registration.pw,
          },
        });
        this.accessToken = res.data;
        console.log(JSON.stringify(this.accessToken));
        console.log("Registration successful");
        this.registration.user = "";
        this.registration.pw = "";
      } catch (e) {
        console.error(e);
      }
    },

    async loginUser() {
      try {
        const res = await axios({
          method: "post",
          url: loginURL,
          data: {
            email: this.login.user,
            password: this.login.pw,
          },
        });

        this.accessToken = res.data;
        console.log(JSON.stringify(this.accessToken));
        console.log("Login successful");
        this.getTodos();
      } catch (e) {
        console.error(e);
      }
    },
    async getTodos() {
      await axios
        .get(baseURL, {
          headers: {
            Authorization: `Bearer ${this.accessToken.access_token}`,
          },
        })
        .then((res) => {
          console.log(res.data);
          this.todos = res.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    checkLoggedIn() {
        console.log(this.accessToken.access_token)
    },
    logOut() {
        this.accessToken = '';
        console.log(this.accessToken.access_token)
    }
  },
};
</script>