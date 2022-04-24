<template>
  <header>
    <div class="container header">
      <div class="row h-100 align-items-center">
        <div class="col-12 text-center">
          <nuxt-link to="/"><h1>Arrose-moi !</h1></nuxt-link>
          <div id="hamburger-menu" class="hamburger">
            <svg
              width="31"
              height="27"
              viewBox="0 0 31 27"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M0 15.4H30.8V11H0V15.4ZM0 26.4H30.8V22H0V26.4ZM0 0V4.4H30.8V0H0Z"
                fill="#141414"
              />
            </svg>
          </div>
        </div>
      </div>
    </div>
    <div id="lateral" class="menu-lateral">
      <nuxt-link id="menu-link" to="/">Ma liste</nuxt-link>
      <nuxt-link id="menu-link" to="/profile">Ajouter</nuxt-link>
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
      </div>  
    </div>-->
  </header>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  computed: {
    ...mapGetters(["isAuthenticated", "loggedInUser"]),
  },
  mounted() {
    // Inside page components
this.$OneSignal.push(() => {
    this.$OneSignal.isPushNotificationsEnabled((isEnabled) => {
    if (isEnabled) {
      console.log('Push notifications are enabled!')
    } else {
      console.log('Push notifications are not enabled yet.')
    }
  })
})

    var hamburger = document.getElementById("hamburger-menu");
    var lateral = document.getElementById("lateral");
    var menu_link = document.getElementById("menu-link");
    let bool = false;
    console.log(hamburger);
    hamburger.onclick = function () {
      if (!bool) {
        lateral.style.visibility = "visible";
      } else {
        lateral.style.visibility = "hidden";
      }
      bool = !bool;
    };
    document.querySelectorAll("[id=menu-link]").forEach(
      (element) =>
        (element.onclick = function () {
          lateral.style.visibility = "hidden";
          console.log("salut");
          bool = false;
        })
    );
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
  background: #ffffff;
  box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.25);
  position: relative;

  a {
    text-decoration: none;
    h1 {
      text-decoration: none;
      color: #141414;
    }
  }
  .col-12 {
    position: relative;
    .hamburger {
      cursor: pointer;
      position: absolute;
      top: 0;
      right: 1rem;
      &:hover svg path {
        fill: #141414;
      }
    }
  }
  .menu-lateral {
    visibility: hidden;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    width: 50%;
    background: white;
    height: 100vh;
    z-index: 1;
    padding-top: 4rem;
    padding-left: 2rem;

    box-shadow: 4px 0px 4px rgba(0, 0, 0, 0.25);

    a {
      display: grid;
      font-size: 2rem;
      font-family: "Rozha One";
      margin-bottom: 1rem;
      color: black;
      &:hover {
        color: #141414;
      }
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