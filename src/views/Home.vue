<template>
  <section class="section-home-welcome">
    <div class="container">
      <div class="main-title">Welcome back to your dashboard</div>
    </div>
  </section>
  <section class="section-home-metrics">
    <div class="container">
      <div class="metric-single-binary">
        <div class="metric-single-binary-title">
          You've set up a tracker to track
          <span class="user-data user-data--to-track">meditation sessions</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";

const baseURL = "http://localhost:3000/todos";

export default {
  name: "App",

  data() {
    return {
      todos: [],
      todoName: "",
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
  },
};
</script>

<style lang="scss">
.section-home-welcome {
  padding: 80px 0;
}

.main-title {
  color: #fff;
  font-size: 30px;
}
</style>