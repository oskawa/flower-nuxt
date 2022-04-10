<template>
  <div class="container contenu">
    <div class="row p-relative">
      <div class="col-12">
        <div class="image">
          <img
            v-if="plante.image"
            v-bind:src="'https://warm-plateau-50892.herokuapp.com' + plante.image.url"
          />
          <img v-if="!plante.image" src="" alt="" />
        </div>
        <h2>{{ plante.nom }}</h2>
        <p><span>Prochain arrosage :</span> {{ plante.dateArrosage }}</p>
        <p>
          <span>Fréquence d'arrosage :</span> Tous les
          {{ plante.frequency }} jours
        </p>
      </div>

      <div class="col-12 menu-flottant">
        <div class="p-relative">
          <button class="arroser" @click="updateTime(plante.id, plante.frequency, plante.dateArrosage)">
            <svg
              width="52"
              height="52"
              viewBox="0 0 52 52"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M26 47.6667C21.4029 47.6667 16.9941 45.8405 13.7435 42.5899C10.4929 39.3392 8.66669 34.9304 8.66669 30.3333C8.66669 22.7457 14.5384 16.6768 19.7015 11.115L26 4.33334L32.2985 11.115C37.4617 16.679 43.3334 22.7478 43.3334 30.3333C43.3334 34.9304 41.5072 39.3392 38.2565 42.5899C35.0059 45.8405 30.5971 47.6667 26 47.6667V47.6667Z"
                stroke="#F8F8F8"
                stroke-width="5"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import { mapGetters } from "vuex";
export default {
  middleware: "auth",
  computed: {
    ...mapGetters(["loggedInUse", "isAuthenticated"]),
  },
  data() {
    return {
      id: this.$route.params.id,
      plante: {
          id:null,
        nom: "",
        image: null,
        description: "",
        frequency: null,
        dateArrosage: null,
      },
    };
  },
  mounted() {
    this.plantesEnregistrees = JSON.parse(
      window.localStorage.getItem("plantes")
    );

    axios
      .get(`https://warm-plateau-50892.herokuapp.com/arrosage-plantes/${this.id}`)
      .then((response) => {
        console.log(response);
        this.plante = {
            id:response.data.id,
          nom: response.data.NomDeLaPlante,
          image: response.data.imageDeLaPlante,
          frequency: response.data.frequency,
          dateArrosage: response.data.dateDarrosage,
        };
      });
  },
  methods: {
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
      const res = await axios.get(`https://warm-plateau-50892.herokuapp.com/users/${user_id}`, {
        headers: {
          Authorization: `Bearer ${window.localStorage.getItem("jwt")}`,
        },
      });
      const user = res.data;

      window.localStorage.setItem("userData", JSON.stringify(user));
      window.localStorage.setItem("plantes", JSON.stringify(user.plantes));

      this.plantesEnregistrees = JSON.parse(
        window.localStorage.getItem("plantes")
      );
      axios
      .get(`https://warm-plateau-50892.herokuapp.com/arrosage-plantes/${this.id}`)
      .then((response) => {
        console.log(response);
        this.plante = {
            id:response.data.id,
          nom: response.data.NomDeLaPlante,
          image: response.data.imageDeLaPlante,
          frequency: response.data.frequency,
          dateArrosage: response.data.dateDarrosage,
        };
      });
    },
  },
};
</script>

<style lang="scss">
.container.contenu {
  height: 88vh;
  .row {
    height: 100%;
    position: relative;
    .menu-flottant {
      position: absolute;
      bottom: 10px;
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 65px;

      .p-relative {
        position: relative;
        height: 100%;
        width: 100%;
        display: flex;
        background: #ffffff;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.25);
        border-radius: 15px;
        button {
          height: 80px;
          width: 80px;
          border: none;
          border-radius: 50%;
          background-color: #47a34a;
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          //   transform: translate(0, %);
          margin-left: 0;
          margin-right: 0;
        }
      }
    }
  }
  .image {
    width: 100%;
    height: 30vh;
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
  h2 {
    font-family: "Rozha One";
    padding-top: 1rem;
  }
  p {
    font-family: "Open Sans";
    span {
      font-weight: 800;
    }
  }
}
</style>