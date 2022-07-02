<template>
  <div class="container contenu">
    <div class="row p-relative">
      <div class="col-12">
        <div class="image">
          <img v-if="plante.image" v-bind:src="plante.image.url" />
          <img v-if="!plante.image" src="" alt="" />
        </div>
        <h2>{{ plante.nom }}</h2>
        <p><span>Prochain arrosage :</span> {{ plante.dateArrosage }}</p>
        <p>
          <span>Fréquence d'arrosage :</span> Tous les
          {{ plante.frequency }} jour(s) !
        </p>
      </div>

      <div class="col-12 menu-flottant">
        <div class="p-relative">
          <button v-if="Date.now() > new Date(plante.dateArrosage)"
            class="arroser"
            @click="
              updateTime(plante.id, plante.frequency, plante.dateArrosage)
            "
          >
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

          <button id="btn_delete" class="delete">
            <svg
              width="110"
              height="116"
              viewBox="0 0 110 116"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M105.188 21.5H86.9375V10.0938C86.9375 5.06074 82.8455 0.96875 77.8125 0.96875H32.1875C27.1545 0.96875 23.0625 5.06074 23.0625 10.0938V21.5H4.8125C2.28887 21.5 0.25 23.5389 0.25 26.0625V30.625C0.25 31.2523 0.763281 31.7656 1.39062 31.7656H10.0023L13.524 106.334C13.7521 111.196 17.7729 115.031 22.6348 115.031H87.3652C92.2414 115.031 96.2479 111.21 96.476 106.334L99.9977 31.7656H108.609C109.237 31.7656 109.75 31.2523 109.75 30.625V26.0625C109.75 23.5389 107.711 21.5 105.188 21.5ZM76.6719 21.5H33.3281V11.2344H76.6719V21.5Z"
                fill="#0D2C17"
              />
            </svg>
          </button>
        </div>
      </div>
      <div id="id_delete" class="pop-delete">
        <h3>Suppression</h3>
        <p>Es tu sûr de vouloir supprimer cette plante ?</p>
        <div class="button-delete">
          <button class="yes-delete" @click="deletePlant(plante.id)">Oui</button>
          <button id="no-delete" class="no-delete">Non</button>
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
        id: null,
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
      .get(
        `https://warm-plateau-50892.herokuapp.com/arrosage-plantes/${this.id}`
      )
      .then((response) => {
        console.log(response);
        this.plante = {
          id: response.data.id,
          nom: response.data.NomDeLaPlante,
          image: response.data.imageDeLaPlante,
          frequency: response.data.frequency,
          dateArrosage: response.data.dateDarrosage,
        };
      });

      var btn_delete = document.getElementById('btn_delete')
      var pop_delete = document.getElementById('id_delete')
      var non_suppr = document.getElementById('no-delete')
      var toggle_visible = false

      btn_delete.onclick = function () {
        if (!toggle_visible) {
          pop_delete.style.visibility="visible"
        }else{
          pop_delete.style.visibility="hidden"
        }
        toggle_visible = !toggle_visible
      }
      non_suppr.onclick = function(){
        pop_delete.style.visibility="hidden"
        toggle_visible = false
      }


  },
  methods: {
     isSameDay(date1, date2){
  return date1.getFullYear() === date2.getFullYear() &&
    date1.getMonth() === date2.getMonth() &&
    date1.getDay() === date2.getDay()
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
       this.$router.push({path: "/"})
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
      axios
        .get(
          `https://warm-plateau-50892.herokuapp.com/arrosage-plantes/${this.id}`
        )
        .then((response) => {
          console.log(response);
          this.plante = {
            id: response.data.id,
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
  padding-top: 1rem;
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
        button.delete {
        
          position: absolute;
          right: 1rem;
          top: 1rem;
          height: 30px;
          width: 30px;
          border: none;
          background-color: white;
          svg {
            width: 100%;
            height: 100%;
          }
        }
        button.arroser {
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
      border-radius: 15px;
    }
  }
  h2 {
    font-family: "Rozha One";
    padding-top: 1rem;
    font-size: 2.5rem;
    text-align: center;
  }
  p {
    font-family: "Open Sans";
    text-align: center;
    span {
      font-weight: 800;
    }
  }
  .pop-delete {
    visibility: hidden;
    position: absolute;
    border-radius: 15px;
    padding: 1rem;

    background-color: white;
    width: 60%;
    height: 30%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    /* Group 8 */

    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    h3 {
      text-align: center;
      width: 100%;
      font-family: "Rozha One";
      font-weight: 700;
    }
    .button-delete {
      display: flex;
      margin: 0 -1rem;
      height:4rem;
      button {
        width: 50%;
        border: none;
        background: white;
        &.yes-delete{
          color: white;
          background: #742222;
        }
      }
    }
  }
}
</style>