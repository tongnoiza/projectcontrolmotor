<template>
  <v-app :light="true">
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-toolbar-title v-text="title" /> &nbsp; &nbsp; &nbsp;
      <v-btn
        elevation="20"
        @click="exportTableToExcel()"
        text
        color="primary"
      >
        <div style="color: azure">
          <span class="mdi mdi-45px mdi-content-save-plus-outline"></span>
          ดาวน์โหลด
        </div>
      </v-btn>
      &nbsp; &nbsp; &nbsp;
      <v-row>
        <v-dialog v-model="dialog" persistent max-width="290">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="error"
              dark
              text
              v-bind="attrs"
              elevation="20"
              v-on="on"
            >
              <div style="color: azure">
                <span class="mdi mdi-archive-remove-outline"></span> ล้างข้อมูล
              </div>
            </v-btn>
          </template>
          <v-card>
            <v-card-title class="text-h5"> ยืนยันการล้างข้อมูล </v-card-title>
            <v-card-text>ต้องการล้างข้อมูลหรือไม่ ?</v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="green darken-1" text @click="dialog = false">
                <span class="mdi mdi-close-thick"></span>
                ยกเลิก
              </v-btn>
              <v-btn
                color="error"
                :loading="loading"
                text
                @click="cleartable()"
              >
                <span class="mdi mdi-check-bold"></span>
                ยืนยัน
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container><Nuxt /></v-container>
    </v-main>
    <v-footer :absolute="!fixed" app>
      <span
        >&copy; {{ new Date().getFullYear() }} Development By B.Sontaya</span
      >
    </v-footer>
  </v-app>
</template>

<script>
import * as XLSX from 'xlsx';
export default {
  head:{
    title:'โปรเจคจบ',
    meta: [
      {
        hid: 'description',
        name: 'description',
        content: 'Home page description'
      }
    ],
  },
  // components: { PopupVue },
  name: "DefaultLayout",
 async created() {
    // this.$nuxt.$on("datalen", (even) => this.delbut(even));
  },


  data() {
    return {
      data:[],
      dataarray:[],
      exportbutton: false,
      delbutton: false,
      del: true,
      dialog: false,
      clipped: false,
      drawer: false,
      fixed: false,
      loading: false,
      items: [
        {
          icon: "mdi-apps",
          title: "Welcome",
          to: "/",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Inspire",
          to: "/inspire",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "ระบบควบคุมมอเตอร์เหนี่ยวนำ",
    };
  },

  methods: {
    delbut(val) {
      this.delbutton = val;
      this.exportbutton = val;
    },
    async cleartable() {
      this.loading = true;
      await this.$axios
        .$get("https://sontaya.000webhostapp.com/del.php")
        .then((res) => {
          console.log(res)
          if (res == 1) {
            this.$nuxt.$emit("delete", "delete successfully");
          }
        });
      this.loading = false;
      this.dialog = false;
    },

   async exportTableToExcel() {
      let sourc = []
      sourc = await this.$axios.$get(
          "https://sontaya.000webhostapp.com/getlog.php"
        );
console.log('sourc ', sourc );
      //  let data = []
      //  data =  sourc.forEach(x=> {
      //   delete x["0"]
      //   delete x["1"]
      //   })
  
  let binaryWS = XLSX.utils.json_to_sheet(sourc); 
  
  // Create a new Workbook
  var wb = XLSX.utils.book_new() 

  // Name your sheet
  XLSX.utils.book_append_sheet(wb, binaryWS, 'History Log') 

  // export your excel
  XLSX.writeFile(wb, 'HistoryLog.xlsx');


    },
  },
};
</script>
