<template>
  <HelloWorld msg="JSON server + Vue 3" />
  <h1>Users:</h1>
  <div class="form-wrap">
    <label for="new-user">Enter new user name: </label>
    <input type="text" name="new-user" v-model="todoName" />
    <button class="submit" @click="addTodo">Add new user</button>
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
      todoName: ""
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

      this.todos = res.data;
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
