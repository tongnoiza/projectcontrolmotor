<template>
  <v-app :light="true">
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-toolbar-title v-text="title" /> &nbsp; &nbsp; &nbsp;
      <v-btn
        @click="exportTableToExcel('data', (filename = 'Motor Log'))"
        text
        color="primary"
      >
        Export data
      </v-btn>
      &nbsp; &nbsp; &nbsp;

      <v-row>
        <v-dialog v-model="dialog" persistent max-width="290">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              text
              v-bind="attrs"
              
              v-on="on"
              >Clear Table
            </v-btn>
          </template>
          <v-card>
            <v-card-title class="text-h5"> ยืนยันการลบข้อมูล </v-card-title>
            <v-card-text>ต้องการลบข้อมูลหรือไม่</v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="green darken-1" text @click="dialog = false">
                Disagree
              </v-btn>
              <v-btn color="green darken-1" text @click="cleartable()">
                Agree
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>

      <v-btn> Clear Table </v-btn>
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container><Nuxt /></v-container>
    </v-main>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }} Development By sontaya</span>
    </v-footer>
  </v-app>
</template>

<script>
import PopupVue from "../components/popup.vue";
export default {
  components: { PopupVue },
  name: "DefaultLayout",
  data() {
    return {
      del: true,
      dialog: false,
      clipped: false,
      drawer: false,
      fixed: false,
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
    async cleartable() {
    
           await this.$axios
        .$get("https://tongza.000webhostapp.com/v.php")
        .then((res) => {
          if (res == 1) {
            this.$nuxt.$emit("delete", "delete successfully");
          }
        });
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
