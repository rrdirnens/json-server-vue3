<template>
  <HelloWorld msg="JSON server + Vue 3" />
  <div style="display: none;" class="create-new-user" @click="addUser">
    REGISTER
  </div>
  <div class="register-user">
    <div class="title">Register please:</div>
    <input
      type="text"
      class="user-name"
      v-model="registration.regUsername"
      placeholder="Username"
    />
    <input
      type="password"
      class="user-password"
      v-model="registration.regPassword"
      placeholder="Password"
    />
    <button class="reg-submit" @click="registerUser">Register</button>
  </div>
  <div style="display: none">
    <h1>Todos:</h1>
    <div class="form-wrap">
      <label for="new-user">Enter new todo: </label>
      <input type="text" name="new-user" v-model="todoName" />
      <button class="submit" @click="addTodo">Submit</button>
    </div>
  </div>

  <ul style="display: none">
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
      todoName: "",
      accessToken: "",
      registration: {
        regUsername: "",
        regPassword: "",
      },
    };
  },
  async created() {
    // try {
    //   const res = await axios({
    //     method: "post",
    //     url: loginURL,
    //     data: {
    //       email: "nilson@email.com",
    //       password: "nilson",
    //     },
    //   });

    //   this.accessToken = res.data;
    //   console.log(JSON.stringify(this.accessToken));
    //   console.log("Login successful");
    //   this.getTodos();
    // } catch (e) {
    //   console.error(e);
    // }
  },
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
            email: this.registration.regUsername,
            password: this.registration.regPassword,
          },
        });

        this.accessToken = res.data;
        console.log(JSON.stringify(this.accessToken));
        console.log("Registration successful");
      } catch (e) {
        console.error(e);
      }
    },

    // async addUser() {
    //   try {
    //     const res = await axios({
    //       method: "post",
    //       url: registerURL,
    //       data: {
    //         email: "rob",
    //         password: "1234",
    //       },
    //     });

    //     this.accessToken = res.data;
    //     console.log(JSON.stringify(this.accessToken));
    //     console.log("Registration successful");
    //   } catch (e) {
    //     console.error(e);
    //   }
    // },

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

.register-user {
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
