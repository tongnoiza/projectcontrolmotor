<template>
  <v-app :light="true">
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-toolbar-title v-text="title" /> &nbsp; &nbsp; &nbsp;
      <v-btn
        v-show="exportbutton"
        elevation="20"
        @click="exportTableToExcel('data', (filename = 'Motor Log'))"
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
              v-show="delbutton"
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
        >&copy; {{ new Date().getFullYear() }} Development By B.sontaya</span
      >
    </v-footer>
  </v-app>
</template>

<script>
import PopupVue from "../components/popup.vue";
export default {
  components: { PopupVue },
  name: "DefaultLayout",
  created() {
    this.$nuxt.$on("datalen", (even) => this.delbut(even));
  },
  async fetch() {
    let data = await this.$axios.post("http://localhost:443/gg", {
      name: "tonguuza",
    });

    console.log("data ", data);
  },
  data() {
    return {
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
      console.log("val ", val);
      this.delbutton = val;
      this.exportbutton = val;
    },
    async cleartable() {
      this.loading = true;
      await this.$axios
        .$get("https://tongza.000webhostapp.com/v.php")
        .then((res) => {
          if (res == 1) {
            this.$nuxt.$emit("delete", "delete successfully");
          }
        });
      this.loading = false;
      this.dialog = false;
    },

    exportTableToExcel(tableID, filename = "") {
      var downloadLink;
      var dataType = "application/vnd.ms-excel";
      var tableSelect = document.getElementById(tableID);
      var tableHTML = tableSelect.outerHTML.replace(/ /g, "%20");
      // Specify file name
      filename = filename ? filename + ".xls" : "excel_data.xls";
      // Create download link element
      downloadLink = document.createElement("a");
      document.body.appendChild(downloadLink);
      if (navigator.msSaveOrOpenBlob) {
        var blob = new Blob(["\ufeff", tableHTML], {
          type: dataType,
        });
        navigator.msSaveOrOpenBlob(blob, filename);
      } else {
        // Create a link to the file
        downloadLink.href = "data:" + dataType + ", " + tableHTML;
        // Setting the file name
        downloadLink.download = filename;
        //triggering the function
        downloadLink.click();
      }
    },
  },
};
</script>
