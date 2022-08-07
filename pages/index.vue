<template>
  <div>
    <v-data-table
 :dense="true"
      id="data"
      :loading="load"
      loading-text="กำลังโหลดข้อมูล..."
      :headers="headers"
      :items="data"
      color="green"
      :items-per-page="10"
      class="elevation-1"
    ></v-data-table>
  </div>
</template>
<script>
export default {
  async fetch() {
    this.data = await this.$axios.$get("https://tongza.000webhostapp.com/");
    this.data.reverse();
    this.load = false;
  },
  data() {
    return {
      t: "",
      msg: [
        { n: "sisaket", d: "l" },
        { n: "ubon", d: "r" },
        { n: "krungthep", d: "l" },
        { n: "korach", d: "l" },
        { n: "mukdahan", d: "l" },
      ],
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
  methods: {},
};
</script>
