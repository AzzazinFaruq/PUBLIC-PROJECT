<template>
  <v-container class="">
    <h1>Halaman Dashboard</h1>
    <v-divider class="mb-3"> </v-divider>
    <p>Selamat Datang, <b>{{ this.data.name }}</b></p>
  </v-container>
  <v-container >
<v-card>
  vtable
</v-card>
  </v-container>
</template>

<script>
import axios from "axios";
import { useRouter } from "vue-router";
useRouter;
export default {
  data() {
    return {
      data: [],
    };
  },
  mounted() {
    this.status();
  },
  methods: {
    status() {
      try {
        axios.get("/api/user").then((res) => {
          console.log(res.data);
          this.data = res.data.data;
          console.log(this.data.level)
          switch(this.data.level){
            case "superAdmin":
            this.$router.push("/adminDash")
              break;
            case "enum":
              break;
            case "admin":
            this.$router.push("/adminDash")
              break;
          }
        });
      } catch (error) {
        console.error(error);
      }

    },
    config(){
      if(this.data.level != 'enum'){
        // this.$router.push("/adminDash")
      }

    }
  },
  created() {
    this.config();
  },
};
</script>
