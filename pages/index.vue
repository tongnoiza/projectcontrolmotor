<template>
  <div>
        <v-chip
        color="green"
        dark
      >
    เครื่องที่ 1 สีเขียว
      </v-chip>
    <v-data-table
    id="data"
      :loading="load"
      loading-text="กำลังโหลดข้อมูล..."
      :headers="headers"
      :items="data"
      :items-per-page="10"
      class="elevation-1"
    >
     <template v-slot:[`item.status`]="{ item }">
      <v-chip
        :color="getColor(item.status)"
        dark
      >
        {{ item.status }}
         
      </v-chip>
    </template>
     <template v-slot:[`item.engin`]="{ item }">
      <v-chip
        :color="getColorengine(item.engin)"
        dark
      >
    
           {{ item.engin }}
      </v-chip>
    </template>
    </v-data-table>
  </div>
</template>
<script>
export default {
  created() {
    this.$nuxt.$on("delete", (even) => this.s(even));
  },
  async fetch() {
    this.data = await this.$axios.$get("https://tongza.000webhostapp.com/");
    if (this.data.length == 0)  this.delbut = false;
    this.$nuxt.$emit("datalen", this.delbut);
    this.data.reverse();
    this.load = false;
  },
  data() {
    return {
      delbut: true,
      e1: 1,
      data: [],
      load: true,
      headers: [
        { text: "เครื่องที่", value: "engin" },
        { text: "สถานะ", value: "status" },
        { text: "วัน/เวลา", value: "time" },
      ],
    };
  },
  inject: {
    theme: {
      default: { isDark: false },
    },
  },
  activated() {
    this.s();
  },
  methods: {
    getColorengine(engin){
      if(engin=='เครื่องที่ 1')return "blue"

    },
    getColor(status) {
      if (status == "เปิด") return "green";
      else if (status == "ปิด") return "red";
    },
    async s() {
      this.data = await this.$axios.$get("https://tongza.000webhostapp.com/");
      if (this.data.length == 0) this.delbut = false;
      this.$nuxt.$emit("datalen", this.delbut);
      this.data.reverse();
      this.load = false;
    },
  },
};
</script>
