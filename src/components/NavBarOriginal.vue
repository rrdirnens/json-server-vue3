<template>
  <div class="nav-bar nav-bar--theme-original">
    <router-link class="nav-bar-link" to="/">Home</router-link>
    <router-link class="nav-bar-link" to="/about">About</router-link>
  </div>
</template>

<script>
export default {
  name: "NavBarOriginal",
};
</script>

<style lang="scss">
.nav-bar {
  padding: 30px 0;

  @at-root .nav-bar-link {
    padding: 0 15px;
    font-size: 20px;
    line-height: 21px;
    text-decoration: none;
    color: #42b983;

    @at-root &:first-child {
      padding-left: 0;
    }
  }
}
</style>
