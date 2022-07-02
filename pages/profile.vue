<template>
  <div class="container add">
    <div class="row">
      <div class="col-12">
        <h5>Photo de votre plante</h5>
                <div class="upload-image__div">
        <div id="preview" class="upload-image__preview">
         
          <input
            type="file"
            id="file"
            ref="myFiles"
            class="custom-file-input upload-image__file"
            @change="previewFiles"
            multiple
          />
                   <img id="output" class="upload-image__preview_img" src="download.png" />

        </div>
        </div>
        <h5>Nom de votre plante</h5>
        <input
          type="text"
          v-model="donnees.name"
          placeholder="Nom de la plante"
        />

        <h5>Fréquence d'arrosage</h5>
        <input
          type="number"
          v-model="donnees.frequency"
          placeholder="Fréquence d'arrosage (En jour)"
        />

        <h5>Dernier arrosage</h5>
        <input
          type="date"
          v-model="donnees.arrosage"
          placeholder="Dernier arrosage"
        />

        <button
          id="position"
          class="buttonPlante ajout"
          v-on:click="validPlantes(donnees)"
        >
          Ajouter
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import axios from "axios";

export default {
  middleware: "auth",
  computed: {
    ...mapGetters(["loggedInUser"]),
  },
  data() {
    return {
      files: [],
      username: "",
      email: "",
      password: "",
      success: null,
      error: null,
      plantesEnregistrees: JSON.parse(window.localStorage.getItem("plantes")),
      donnees: {
        name: null,
        frequency: null,
        arrosage: null,
      },
    };
  },
  methods: {
    previewFiles(event) {
      var output = document.getElementById('output');
                            output.src = URL.createObjectURL(event.target.files[0]);
                            output.onload = function() {
                                URL.revokeObjectURL(output.src) // free memory
                            }
    },
    testPlante(item) {
      var frequency = parseInt(item.frequency);

      console.log(PlanteArrosage);
    },
    async validPlantes(item) {
      if (window.localStorage.getItem("auth.strategy")) {
        var PlanteName = item.name;
        // var PlanteArrosage = item.arrosage;

        var frequency = parseInt(item.frequency);
        var PlanteArrosage = new Date(item.arrosage);
        //  console.log(PlanteArrosage.getDate()+ frequency)
        PlanteArrosage.setDate(PlanteArrosage.getDate() + frequency);

        var user_id = JSON.parse(window.localStorage.getItem("userData")).id;

        let bookmarkPlant;

        if (
          this.plantesEnregistrees.findIndex(
            (plant) => plant.nomDeLaPlante === item.name
          ) === -1
        ) {
          bookmarkPlant = {
            nomDeLaPlante: PlanteName,
            frequency: frequency,
            users_permissions_users: user_id,
            arrosage: PlanteArrosage,
          };
          this.plantesEnregistrees.push(bookmarkPlant);
          window.localStorage.setItem(
            "plantes",
            JSON.stringify(this.plantesEnregistrees)
          );
          await axios
            .post(
              `https://warm-plateau-50892.herokuapp.com/arrosage-plantes`,
              {
                NomDeLaPlante: PlanteName,
                frequency: frequency,
                users_permissions_user: user_id,
                dateDarrosage: PlanteArrosage,
              },
              {
                headers: {
                  Authorization: `Bearer ${window.localStorage.getItem("jwt")}`,
                },
              }
            )
            .then((res) => {
              return res.data;
            })
            .then((refId) => {
              let formData = new FormData();
              formData.append("files", this.files[0]);
              formData.append("refId", refId.id);
              formData.append("ref", "arrosage-plantes");
              formData.append("field", "imageDeLaPlante");

              axios.post(
                "https://warm-plateau-50892.herokuapp.com/upload",
                formData
              );
              return refId;
            })
            .then((res) => {
              const val = document.querySelector("input");
              val.value = "";
            })
            .catch((error) => {});

          const res = await axios.get(
            `https://warm-plateau-50892.herokuapp.com/users/${user_id}`,
            {
              headers: {
                Authorization: `Bearer ${window.localStorage.getItem("jwt")}`,
              },
            }
          );
          const user = res.data;
          console.log(user);

          window.localStorage.setItem("userData", JSON.stringify(user));
          window.localStorage.setItem("plantes", JSON.stringify(user.plantes));

          this.$router.push({ path: "/" });
        }
      }
    },
  },
};
</script>


<style lang="scss">
.container.add {
  margin-top: 3rem;
  h5 {
    font-size: 16px;
    font-weight: 800;
    margin-bottom: 1rem;
  }
  input {
    width: 100%;
    background: #ffffff;
    border: none;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    border-radius: 34px;
    margin-bottom: 2rem;
    padding-left: 1rem;
    min-height: 30px;
    padding: 1rem;
    
  }
 .custom-file-input{
  border:none;
  box-shadow: none;
  background:transparent;
 opacity:0;
 margin-bottom:0;
 }
  .buttonPlante {
    min-height: 50px;
    width: 100%;
    background: #fffef8;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    border-radius: 34px;
    border: none;
    &:hover {
      background: #204125;
      color: white;
    }
  }
  .upload-image__preview {
    position: relative;
    background: #204125;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 200px;
    cursor: pointer;
    margin-bottom: 1rem;
  }

  .upload-image__file {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    cursor: pointer;
  }

  .upload-image__preview_img {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    margin: auto;
    cursor: pointer;
    pointer-events: none;
    max-height: 100%;
  }
}
</style>