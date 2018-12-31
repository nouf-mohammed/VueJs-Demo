<template>
  <div id="app">
    <nav id="nav" class="navbar navbar-expand-lg navbar-dark bg-dark">

      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">

        <router-link class="btn btn-success" to="/login" v-if="!isLoggedIn()">تسجيل الدخول</router-link>
        <router-link class="btn btn-danger" to="/logout" v-if="isLoggedIn()">خروج</router-link>
        <!--<form class="form-inline my-2 my-lg-0">
          <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
          <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
        </form>-->
      </div>

    </nav>

    <router-view/>
  </div>
</template>

<script>
    import {isLoggedIn} from "@/services/UserService";

    export default {
      methods: {
          isLoggedIn() {
              return isLoggedIn();
          }
      }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  margin-bottom: 20px;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
