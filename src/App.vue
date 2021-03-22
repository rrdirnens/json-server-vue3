<template>
<!-- <HelloWorld msg="Private tracking" /> -->
<div class="container">
 <NavBarOriginal/>
</div>

<div class="main-content">
  <router-view/>
</div>

</template>

<script>

import NavBarOriginal from './components/NavBarOriginal'

export default {
  name: "App",
  components: {
    NavBarOriginal
  }
}

</script>

<style lang="scss">

@import "scss/app";
@import "bootstrap/scss/bootstrap-grid";

</style>
