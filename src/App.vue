<template>
  <HelloWorld msg="JSON server + Vue 3" />
  <h1>Todos:</h1>
  <div class="form-wrap">
    <label for="new-user">Enter new todo: </label>
    <input type="text" name="new-user" v-model="todoName" />
    <button class="submit" @click="addTodo">Submit</button>
  </div>

  <ul>
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

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      todos: [],
      todoName: "",
      accessToken: ''
    };
  },
  async created() {
    try {
      const res = await axios({
        method: 'post',
        url: loginURL, 
        data: {
          "email": "nilson@email.com",
          "password":"nilson"
        }
      })

      this.accessToken = res.data;
      console.log(JSON.stringify(this.accessToken))
      console.log("Login successful")
      this.getTodos()
    } catch (e) {
      console.error(e);
    }
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

    async getTodos() {
      await axios.get(baseURL, {
        headers: {
          'Authorization': `Bearer ${this.accessToken}`
        }
      })
      .then((res) => {
        console.log(res.data)
        this.todos = res.data
      })
      .catch((error) => {
        console.error(error)
      })
    }
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
