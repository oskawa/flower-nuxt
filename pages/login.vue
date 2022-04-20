<template>
  <section id="login">
    <div class="container">
      <h2 class="title has-text-centered">Connecte toi</h2>

      <Notification v-if="error" type="danger" :message="error" />

      <form method="post" @submit.prevent="login">
        <div class="field">
          <div class="control">
            <input
              v-model="email"
              type="email"
              class="input"
              name="email"
              placeholder="Adresse mail"
            />
          </div>
        </div>
        <div class="field">
          <div class="control">
            <input
              v-model="password"
              type="password"
              class="input"
              name="password"
              placeholder="Mot de passe"
            />
          </div>
        </div>
        <div class="control">
          <button type="submit" class="button is-dark">Se connecter</button>
        </div>
      </form>
      <div class="forgot-pass" style="margin-top: 20px">
        <p>
          <nuxt-link to="/forgot-password">Mot de passe oublié</nuxt-link>
        </p>
      </div>
    </div>
  </section>
</template>

<script>
import Notification from "~/components/Notification";

export default {
  components: {
    Notification,
  },
  data() {
    return {
      email: "",
      password: "",
      error: null,
      data: {},
    };
  },
  methods: {
    async login() {
      this.error = null;

      try {
        const res = await this.$auth.loginWith("local", {
          data: {
            identifier: this.email,
            password: this.password,
          },
        });
        const { jwt, user } = res.data;
        window.localStorage.setItem("jwt", jwt);
        window.localStorage.setItem("userData", JSON.stringify(user));
        window.localStorage.setItem("plantes", JSON.stringify(user.plantes));
      } catch (e) {
        console.log(e);
        this.error = e.response.data.message[0].messages[0].message;
      }
    },
  },
};
</script>


<style lang="scss">
section#login {
  background: #0d2c17;
  height: 88vh;
  h2 {
    font-family: "Rozha One";
    font-style: normal;
    font-weight: 400;
    font-size: 96px;
    line-height: 68px;
    text-align: right;
    text-transform: uppercase;
    color: #ffffff;
  }
  p {
    font-family: "Open Sans";
    font-style: normal;
    font-weight: 300;
    font-size: 20px;
    line-height: 100%;
    /* or 20px */

    color: #ffffff;
  }
  form {
    input {
      background: #ffffff;
      box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
      border-radius: 34px;
      border: none;
      padding: 0.8rem 1rem;
      margin-bottom: 13px;
      width: 100%;
    }
    .control {
      button {
        margin-top: 4rem;
        width: 100%;
        border: none;
        background: #fffef8;
        box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
        border-radius: 34px;
        padding: 0.8rem 1rem;
      }
    }
  }
  .forgot-pass {
    text-align: center;
    a {
      color: white;
      text-decoration: none;
    }
  }
}
</style>