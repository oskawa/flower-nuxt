<template>
  <div class="container add">
    <div class="row">
      <div class="col-12">
        <input
          type="file"
          id="file"
          ref="myFiles"
          class="custom-file-input"
          @change="previewFiles"
          multiple
        />

        <input type="text" v-model="donnees.name" placeholder="Nom de la plante"/>
        <input type="number" v-model="donnees.frequency" placeholder="Fréquence d'arrosage" />

        <input type="date" v-model="donnees.arrosage" placeholder="Dernier arrosage" />

        <button
          id="position"
          class="buttonMovie ajout"
          v-on:click="validPlantes(donnees)"
        >
        Ajouter
          <svg
            width="34"
            height="34"
            viewBox="0 0 34 34"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M23.3113 10.5613C23.6106 10.2763 24.0089 10.1187 24.4222 10.1218C24.8355 10.125 25.2315 10.2885 25.5264 10.578C25.8214 10.8675 25.9924 11.2603 26.0034 11.6734C26.0143 12.0866 25.8643 12.4878 25.585 12.7925L17.1062 23.3963C16.9605 23.5533 16.7845 23.6793 16.5889 23.7668C16.3933 23.8543 16.182 23.9014 15.9678 23.9054C15.7535 23.9093 15.5407 23.87 15.342 23.7899C15.1433 23.7097 14.9628 23.5903 14.8112 23.4388L9.1885 17.816C9.03191 17.6701 8.90632 17.4941 8.81921 17.2986C8.73211 17.1031 8.68527 16.8921 8.68149 16.6781C8.67772 16.4641 8.71708 16.2516 8.79724 16.0531C8.8774 15.8547 8.9967 15.6744 9.14804 15.523C9.29938 15.3717 9.47965 15.2524 9.6781 15.1722C9.87655 15.0921 10.0891 15.0527 10.3031 15.0565C10.5171 15.0603 10.7281 15.1071 10.9236 15.1942C11.1191 15.2813 11.2951 15.4069 11.441 15.5635L15.8907 20.0111L23.2709 10.608C23.2842 10.5916 23.2984 10.576 23.3134 10.5613H23.3113Z"
              fill="#F8F8F8"
            />
          </svg>
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
    previewFiles() {
      this.files = this.$refs.myFiles.files;
     
    },
    testPlante(item){
 
      
      var frequency = parseInt(item.frequency);
     
      console.log(PlanteArrosage);
        
       

    },
    async validPlantes(item) {
      if (window.localStorage.getItem("auth.strategy")) {
        var PlanteName = item.name;
        // var PlanteArrosage = item.arrosage;
        
        var frequency = parseInt(item.frequency);
         var PlanteArrosage = new Date(item.arrosage) ;
      //  console.log(PlanteArrosage.getDate()+ frequency)
      PlanteArrosage.setDate(PlanteArrosage.getDate()+ frequency);



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
      
             const val = document. querySelector('input')
             val.value = ""

            })
            .catch((error) => {
             
            });

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

          this.$router.push({path: "/"})
        }
      }
    },
  },
};
</script>


<style lang="scss">
.container.add {
  margin-top: 3rem;
  input {
    width: 100%;
    background: #ffffff;
    border: none;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    border-radius: 34px;
    margin-bottom: 2rem;
    padding-left: 1rem;
  }
  .buttonMovie{
width:100%;
background: #FFFEF8;
box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
border-radius: 34px;
border: none;

  }
}
</style>