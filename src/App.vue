<template>
  <HelloWorld msg="Private tracking" />
  <div style="display: none" class="create-new-user" @click="addUser">
    REGISTER
  </div>





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


  <div class="login-user" style="margin-top: 20px;">
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





  <div style="">
    <h1>Todos:</h1>
    <div class="form-wrap">
      <label for="new-user">Enter new todo: </label>
      <input type="text" name="new-user" v-model="todoName" />
      <button class="submit" @click="addTodo">Submit</button>
    </div>
  </div>

  <ul style="">
    <li v-for="todo of todos" :key="todo.id">
      {{ todo.todo }}
    </li>
  </ul>



</template>

<script>
import axios from "axios";
import HelloWorld from "./components/HelloWorld.vue";

const baseURL = "http://localhost:3000/todos";
const loginURL = "http://localhost:3000/auth/login";
const registerURL = "http://localhost:3000/auth/register";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      todos: [],
      todoName: '',
      accessToken: '',
      registration: {
        user: '',
        pw: '',
      },
      login: {
        user: '',
        pw: ''
      }
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
        this.registration.user = ''
        this.registration.pw = ''
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
            Authorization: `Bearer ${this.accessToken}`,
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

.register-user, .login-user {
  display: flex;
  flex-direction: column;
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
