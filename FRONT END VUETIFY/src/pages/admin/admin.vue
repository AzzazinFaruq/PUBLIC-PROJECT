<template>
  <v-container fluid class="m">
    <h1>Admin Dashboard</h1>
    <v-divider class="my-2"></v-divider>
    <div class="mt-5">
      <v-row>
        <card
      icon="mdi-home"
      title="Keluarga"
      description="Total Keluarga"
      :count="keluarga"
      card="brown-lighten-1"
      color="brown-lighten-2"

      />
      <card
      icon="mdi-account"
      title="Penduduk"
      description="Total Penduduk"
      :count="penduduk"
      card="light-blue-darken-2"
      color="light-blue-darken-1 text-white"
      />
      <card
      icon="mdi-account-check"
      title="Aktif"
      description="Penduduk Total"
      :count="stat.aktif"
      card="green-lighten-1"
      color="green-lighten-2 text-white"
      />
      <card
      icon="mdi-account-cancel"
      title="Inaktif"
      description="Penduduk Total"
      :count="stat.inaktif"
      color="red-accent-2"
      card="red-lighten-1 text-white"
      />
      </v-row>
    </div>
    <div class="mt-5">
      <v-card class="pa-2">
      <h3 class="ma-2">DATA TERBARU</h3>
      <v-divider></v-divider>
    <v-tabs
      v-model="tab"
      variant="outlined"
      bg-color="deep-green-accent-4"
    >
      <v-tab value="one">DATA KELUARGA</v-tab>
      <v-tab value="two">DATA PENDUDUK</v-tab>
    </v-tabs>

    <v-card-text>
      <v-tabs-window v-model="tab">
        <v-tabs-window-item value="one">
        <dtTable
        :dthead="head"
        :dtbody="dtkel"
        />
        </v-tabs-window-item>
        <v-tabs-window-item value="two">
          Two
        </v-tabs-window-item>

        <v-tabs-window-item value="three">
          Three
        </v-tabs-window-item>
      </v-tabs-window>
    </v-card-text>
  </v-card>
    </div>

  </v-container>
</template>

<script>
import axios from 'axios'
import card from '@/components/card.vue';
import dtTable from '@/components/dtTable.vue';
export default {
  components:{
    card,dtTable
  },
  data() {
    return {
      head:[
        {id:0, name:'NOMOR KK'},
        {id:1, name:'NOMOR NIK'},
        {id:2, name:'NAMA'},
        {id:3, name:'STATUS'},
        {id:4, name:'USER'}
      ],
      keluarga:0,
      penduduk:'',
      stat:[],
      tab: null,
      data: [],
      level:"none",
      dtkel:[]
    };
  },
  mounted() {
    this.status();
    this.datalast();
    this.counter();
    this.alive();
  },
  methods: {
    alive(){
      axios.get('/api/alive')
      .then((res)=>{
        this.stat=res.data.data;
      })
    },
    counter(){
      axios.get('/api/jumlah')
      .then((res)=>{
        console.log(res.data)
        this.keluarga=res.data.keluarga;
        this.penduduk=res.data.penduduk;
      })
    },
    status() {
      try {
        axios.get("/api/user").then((res) => {
          this.data = res.data.data;
          this.level = res.data.data.level
          switch(this.level){
            case "enum":
              this.$router.push("/forbidden")
              break;
            default:
              break;
          }
        });
      } catch (error) {
        console.error(error);
      }
    },
    datalast(){
      axios.get('/api/latestkel')
      .then((res)=>{
        console.log(res.data)
        this.dtkel = res.data;
      })
    }
  },
  created() {

  },
};

</script>

<style>

</style>
