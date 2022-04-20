<template>
  <section id="section-register">
    <div class="container h-100">
      
          <h2 class="title has-text-centered">Inscris toi !</h2>
          <p>
            Un accès rapide et immédiat, l’inscription est nécessaire seulement
            pour la sauvegarde des tes plantes !
          </p>
          <Notification v-if="success" type="success" :message="success" />
          <Notification v-if="error" type="danger" :message="error" />

          <form v-if="!success" method="post" @submit.prevent="register">
            <div class="field">
              <div class="control">
                <input
                  v-model="username"
                  type="text"
                  class="input"
                  name="username"
                  placeholder="Pseudo"
                  required
                />
              </div>
            </div>
            <div class="field">
              <div class="control">
                <input
                  v-model="email"
                  type="email"
                  class="input"
                  name="email"
                  placeholder="Adresse mail"
                  required
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
                  required
                />
              </div>
            </div>
            <div class="control">
              <button type="submit" class="button is-dark is-fullwidth">
                Register
              </button>
            </div>
          </form>
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
      username: "",
      email: "",
      password: "",
      success: null,
      error: null,
    };
  },
  methods: {
    async register() {
      this.error = null;
      try {
        this.$axios.setToken(false);
        await this.$axios.post("auth/local/register", {
          username: this.username,
          email: this.email,
          password: this.password,
        });
        this.success = `A confirmation link has been sent to your email account. \
 Please click on the link to complete the registration process.`;
      } catch (e) {
        this.error = e.response.data.message[0].messages[0].message;
      }
    },
  },
};
</script>


<style lang="scss">
section#section-register {
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
        width:100%;
        border: none;
        background: #fffef8;
        box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
        border-radius: 34px;
        padding: 0.8rem 1rem;
      }
    }
  }
}
</style>