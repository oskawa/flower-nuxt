<template>
  <header>
    <div class="container header">
      <div class="row h-100 align-items-center">
        <div class="col-12 text-center">
          <nuxt-link to="/"><h1>Arrose-moi !</h1></nuxt-link>
        </div>
      </div>
    </div>

    <!-- <div id="navbarBasicExample" class="navbar-menu">
      <div v-if="isAuthenticated" class="navbar-start">
        <div class="navbar-item has-dropdown is-hoverable">
          <a class="navbar-link">
            {{ loggedInUser.username }}
          </a>

          <div class="navbar-dropdown">
            <a class="navbar-item" href="/profile">My Profile</a>
            <hr class="navbar-divider" />
            <a class="navbar-item" @click="logout">Logout</a>
          </div>
        </div>
      </div>

      <div v-if="!isAuthenticated" class="navbar-end">
        <div class="navbar-item">
          <div class="buttons">
            <nuxt-link class="button is-primary" to="/register">
              <strong>Register</strong>
            </nuxt-link>
            <nuxt-link class="button is-light" to="/login"> Log in </nuxt-link>
          </div>
        </div>
      </div>  -->
    </div>
  </header>
</template>

<script>
import { mapGetters } from "vuex";


export default {
  computed: {
    ...mapGetters(["isAuthenticated", "loggedInUser"]),
  },
  mounted() {
   
   
   
  },
  methods: {
    async logout() {
      await this.$auth.logout();
      window.localStorage.removeItem("jwt");
      window.localStorage.removeItem("userData");
    },
  },
};
</script>

<style lang="scss">
header {
  a {
    text-decoration: none;
    h1 {
      text-decoration: none;
      color: #141414;
    }
  }
}
.header {
  height: 12vh;
  background: #ffffff;
  box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.25);
  h1 {
    font-family: "Rozha One";
  }
}
</style>