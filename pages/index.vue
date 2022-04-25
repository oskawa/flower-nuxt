<template>
  <section class="index">
    <div v-if="isAuthenticated" class="container h-100 background-white">
      <div class="row h-100 align-items-center">
        <div class="col-12 text-center col-plantes">
          <div v-if="!plantesEnregistrees || plantesEnregistrees.length == 0">
            <h2>Votre liste est vide !</h2>
            <nuxt-link class="add-flower" to="/profile"
              >Ajoutez dès maintenant en cliquant ici !</nuxt-link
            >
          </div>

          <ul v-if="plantesEnregistrees" class="listePlantes">
            <li v-for="item in plantesEnregistrees" :key="item.id">
              <nuxt-link :to="`/plantes/${item.id}`">
                <div class="imagePlante">
                  <img
                    v-if="item.imageDeLaPlante"
                    v-bind:src="item.imageDeLaPlante.url"
                  />
                  <img v-if="!item.imageDeLaPlante" />
                </div>
                <div class="description">
                  <h2>{{ item.NomDeLaPlante }}</h2>
                  <p>
                    <svg
                      width="20"
                      height="11"
                      viewBox="0 0 20 11"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M19.98 7H20H19.98ZM3.35 6C4.54 6 4.77 7 6.68 7C8.63 7 8.77 6 10.01 6C11.2 6 11.43 7 13.34 7C15.29 7 15.43 6 16.67 6C17.86 6 18.07 6.98 19.98 7V5C18.79 5 18.56 4 16.65 4C14.7 4 14.56 5 13.32 5C12.13 5 11.9 4 9.99 4C8.04 4 7.9 5 6.66 5C5.47 5 5.24 4 3.33 4C1.38 4 1.24 5 0 5V7C1.9 7 2.17 6 3.35 6ZM16.67 8C14.72 8 14.58 9 13.34 9C12.15 9 11.92 8 10.01 8C8.06 8 7.91 9 6.67 9C5.43 9 5.29 8 3.34 8C1.39 8 1.24 9 0 9V11C1.95 11 2.11 10 3.34 10C4.58 10 4.72 11 6.67 11C8.62 11 8.77 10 10.01 10C11.2 10 11.43 11 13.34 11C15.28 11 15.43 10 16.67 10C17.86 10 18.09 11 20 11V9C18.76 9 18.62 8 16.67 8ZM3.35 2C4.54 2 4.77 3 6.68 3C8.63 3 8.77 2 10.01 2C11.2 2 11.43 3 13.34 3C15.29 3 15.43 2 16.67 2C17.86 2 18.07 2.98 19.98 3V1C18.79 1 18.56 0 16.65 0C14.7 0 14.56 1 13.32 1C12.13 1 11.9 0 9.99 0C8.04 0 7.9 1 6.66 1C5.47 1 5.24 0 3.33 0C1.38 0 1.24 1 0 1V3C1.9 3 2.17 2 3.35 2Z"
                        fill="#141414"
                      />
                    </svg>

                    <span>Prochain arrosage :</span> {{ item.dateDarrosage }}
                  </p>
                  <p>
                    <svg
                      width="18"
                      height="18"
                      viewBox="0 0 18 18"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M9 0.25C4.16797 0.25 0.25 4.16797 0.25 9C0.25 13.832 4.16797 17.75 9 17.75C13.832 17.75 17.75 13.832 17.75 9C17.75 4.16797 13.832 0.25 9 0.25ZM12.4473 11.6895L11.8887 12.4512C11.8765 12.4678 11.8612 12.4818 11.8436 12.4924C11.826 12.5031 11.8065 12.5102 11.7862 12.5132C11.7659 12.5163 11.7451 12.5154 11.7252 12.5104C11.7052 12.5055 11.6864 12.4966 11.6699 12.4844L8.43945 10.1289C8.41933 10.1145 8.40297 10.0954 8.39175 10.0733C8.38054 10.0512 8.37479 10.0267 8.375 10.002V4.625C8.375 4.53906 8.44531 4.46875 8.53125 4.46875H9.4707C9.55664 4.46875 9.62695 4.53906 9.62695 4.625V9.45898L12.4121 11.4727C12.4824 11.5215 12.498 11.6191 12.4473 11.6895Z"
                        fill="#141414"
                      />
                    </svg>
                    <span>Fréquence d'arrosage :</span> Tous les
                    {{ item.frequency }} jours
                  </p>
                </div>

                <!-- <button
                @click="updateTime(item.id, item.frequency, item.dateDarrosage)"
              >
                Update Time
              </button>
              <button @click="deletePlant(item.id)">Delete</button> -->
              </nuxt-link>

              <!--  -->
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div v-if="!isAuthenticated" class="container h-100">
      <div class="row h-100 align-items-center">
        <div class="col-12">
          <h1>Arrose moi !</h1>
          <p>
            Gérez vos plantes et leurs arrosages au même endroit ! Voyez leur
            évolution et prenez des notes pour le futur “Vous” !
          </p>

          <div class="button-bottom">
            <nuxt-link to="/register" class="inscription btn_fill"
              >Je m'inscris !</nuxt-link
            >
            <nuxt-link to="/login" class="inscription btn_standard"
              >J'ai un compte!</nuxt-link
            >
          </div>

          <!-- <Notification v-if="error" type="danger" :message="error" />
          <form method="post" @submit.prevent="login">
            <div class="field">
              
              <div class="control">
                <input
                  v-model="email"
                  type="email"
                  class="input"
                  name="email"
                  placeholder="E-mail"
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
          </form> -->
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { mapGetters } from "vuex";
import axios from "axios";
import Notification from "~/components/Notification";

export default {
  computed: {
    ...mapGetters(["loggedInUse", "isAuthenticated"]),
  },
  components: {
    Notification,
  },
  data() {
    return {
      plantesEnregistrees: [],
      email: "",
      password: "",
      error: null,
      data: {},
    };
  },
  mounted() {
    console.log(this.plantesEnregistrees.length);
    if (window.localStorage.getItem("userData")) {
      this.update();
      var user_id = JSON.parse(window.localStorage.getItem("userData")).id;
      var plantesEnregistrees = JSON.parse(
        window.localStorage.getItem("plantes")
      );
      var date = [];
      plantesEnregistrees.forEach((individuel) =>
        date.push(individuel.dateDarrosage)
      );
      console.log(date);
      const sort = date.sort((a,b)=>b - a)
      console.log(sort[0])
      var date_uniq = new Date(sort[0])
      console.log(date_uniq)
       let timestamp = Math.floor(date_uniq / 1000);
       console.log(timestamp)
    }
    this.$OneSignal.push(() => {

      this.$OneSignal.setExternalUserId('user_id');
      this.$OneSignal.deleteTag("date");
      this.$OneSignal.sendTag("date", timestamp);
      this.$OneSignal.isPushNotificationsEnabled((isEnabled) => {
        if (isEnabled) {
          console.log("Push notifications are enabled!");
        } else {
          console.log("Push notifications are not enabled yet.");
        }
      });
    });
  },
  methods: {
    async update() {
      var user_id = JSON.parse(window.localStorage.getItem("userData")).id;
      const res = await axios.get(
        `https://warm-plateau-50892.herokuapp.com/users/${user_id}`,
        {
          headers: {
            Authorization: `Bearer ${window.localStorage.getItem("jwt")}`,
          },
        }
      );
      const user = res.data;

      window.localStorage.setItem("userData", JSON.stringify(user));
      window.localStorage.setItem("plantes", JSON.stringify(user.plantes));
      this.plantesEnregistrees = JSON.parse(
        window.localStorage.getItem("plantes")
      );
    },
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
    async deletePlant(id) {
      var user_id = JSON.parse(window.localStorage.getItem("userData")).id;
      await axios
        .delete(
          `https://warm-plateau-50892.herokuapp.com/arrosage-plantes/${id}`
        )
        .then(
          (response) => {
            console.log(response);
          },
          {
            headers: {
              Authorization: `Bearer ${window.localStorage.getItem("jwt")}`,
            },
          }
        );
      const res = await axios.get(
        `https://warm-plateau-50892.herokuapp.com/users/${user_id}`,
        {
          headers: {
            Authorization: `Bearer ${window.localStorage.getItem("jwt")}`,
          },
        }
      );
      const user = res.data;
      console.log(user.plantes);

      window.localStorage.setItem("userData", JSON.stringify(user));
      window.localStorage.setItem("plantes", JSON.stringify(user.plantes));

      this.plantesEnregistrees = JSON.parse(
        window.localStorage.getItem("plantes")
      );
      this.$router.push({ path: "/" });
    },
    async updateTime(id, frequency, arrosage) {
      var user_id = JSON.parse(window.localStorage.getItem("userData")).id;

      var test = new Date(arrosage);
      test.setDate(test.getDate() + frequency);
      console.log(test);

      await axios.put(
        `https://warm-plateau-50892.herokuapp.com/arrosage-plantes/${id}`,
        {
          dateDarrosage: test,
        },
        {
          headers: {
            Authorization: `Bearer ${window.localStorage.getItem("jwt")}`,
          },
        }
      );
      const res = await axios.get(
        `https://warm-plateau-50892.herokuapp.com/users/${user_id}`,
        {
          headers: {
            Authorization: `Bearer ${window.localStorage.getItem("jwt")}`,
          },
        }
      );
      const user = res.data;

      window.localStorage.setItem("userData", JSON.stringify(user));
      window.localStorage.setItem("plantes", JSON.stringify(user.plantes));

      this.plantesEnregistrees = JSON.parse(
        window.localStorage.getItem("plantes")
      );
    },
  },
};
</script>

<style lang="scss">
section.index {
  background: #0d2c17;
  height: 88vh;
  position: relative;
  .background-white {
    background-color: white !important;
    background-image: url("/assets_plante.png");
    background-repeat: no-repeat;
  }
  .button-bottom {
    position: fixed;
    bottom: 2rem;
    display: grid;
    text-align: center;
    width: 100%;
    justify-content: center;
    align-items: center;
    .btn_fill {
      padding: 1rem 0;
      background: white;
      color: #0d2c17;
      border-radius: 20px;
      text-decoration: none;
      width: 300px;
    }
    .btn_standard {
      text-decoration: none;
      color: white;
      margin-top: 2rem;
    }
  }
  h1 {
    font-family: "Rozha One";

    font-style: normal;
    font-weight: 400;
    font-size: 60px;
    line-height: 68px;
    text-align: right;
    text-transform: uppercase;
    color: #ffffff;
  }
  form {
    input {
      width: 100%;
      border: none;
      border-radius: 15px;
      box-shadow: 0px 0px 7px rgba(0, 0, 0, 0.25);
      height: 2.5rem;
      padding-left: 1rem;
      margin-bottom: 1rem;
      background-color: white;
    }
    button {
      width: 100%;
      border: none;
      border-radius: 15px;
      box-shadow: 0px 0px 7px rgba(0, 0, 0, 0.25);
      background-color: #47a34a;
      color: white;
      height: 3.5rem;
    }
  }
  .add-flower {
    text-decoration: none;
    color: white;
    background: #0d2c17;
    border-radius: 15px;
    padding: 0.5rem 1rem;
    margin-top: 1rem;
  }
  h2 {
    font-family: "Rozha One";
    padding-top: 1rem;
  }
  p {
    text-align: center;
    color: white;
  }
}
ul.listePlantes {
  background-image: url("/assets_plante.png");
  list-style: none;
  padding: 0;
  li {
    height: 142px;
    background: #ffffff;
    box-shadow: 0px 0px 7px rgba(0, 0, 0, 0.25);
    border-radius: 15px;
    margin-bottom: 1.5rem;
    a {
      display: flex;
      height: 100%;
      text-decoration: none;
    }
    cursor: pointer;
    .imagePlante {
      height: 100%;
      width: 30%;
      display: flex;
      justify-content: center;
      align-items: center;
      img {
        height: 90%;
        width: 90%;
        object-fit: cover;

        border-radius: 15px;
      }
    }
    .description {
      text-align: left;
      padding-left: 15px;
      h2 {
        color: #141414;
      }
      p {
        text-align: left;
        font-family: "Open Sans";
        font-style: normal;
        font-weight: 400;
        font-size: 13px;
        line-height: 18px;
        color: #000000;
        margin-bottom: 0;
        span {
          font-weight: bold;
        }
      }
    }
  }
}
</style>