<template>
  <div>
    <v-row>
      <v-col></v-col>
      <v-col cols="8">
        <v-data-table id="data" :loading="load" loading-text="กำลังโหลดข้อมูล..." :headers="headers" :items="data"
          :items-per-page="5" class="elevation-1">
          <template v-slot:[`item.log`]="{ item }">
            <v-chip :color="getColor(item.log)">
              {{ item.log }}

            </v-chip>
          </template>
        </v-data-table>
      </v-col>
      <v-col></v-col>
    </v-row>


  </div>
</template>
<script>
export default {
  created() {
    this.$nuxt.$on("delete", (even) => this.pulldata(even));
  },
  async fetch() {
    this.data = await this.$axios.$get("https://sontaya.000webhostapp.com/getlog.php");
    this.data.reverse()
    if (this.data.length == 0) this.delbut = false;
    this.$nuxt.$emit("datalen", this.delbut);
    this.load = false;
  },
  data() {
    return {
      delbut: true,
      e1: 1,
      data: [],
      load: true,
      headers: [
        { text: "เครื่องที่/สถานะ", value: "log" },
        { text: "วัน/เวลา", value: "Datetime" },
      ],
    };
  },
  inject: {
    theme: {
      default: { isDark: false },
    },
  },

  methods: {
    async pulldata(even) {
      console.log('event ', even)
      this.data = await this.$axios.$get("https://sontaya.000webhostapp.com/getlog.php");
      this.data.reverse()
    },
    getColor(status) {
      if (status.includes('ขัดข้อง')) return "red";
      else if (status.includes('เปิด')) return "green";
      else if (status.includes('ปิด')) return "black";
    },
  },
};
</script>
