<template>
  <div>   <v-chip
        color="green"
        dark
      >
    1 
      </v-chip>
          <v-chip
        color="orange"
        dark
      >
    2
      </v-chip>  <v-chip
        color="red"
        dark
      >
   3 
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
      <div v-if="item.status!='ขัดข้อง'">
        {{ item.status }}
      </div>
      <v-chip v-else
        :color="getColor(item.status)"
     
      >
        {{ item.status }}
         
      </v-chip>
    </template>
     <!-- <template v-slot:[`item.engin`]="{ item }">
      <v-chip
        :color="getColorengine(item.engin)"
        dark
      >
    
           {{ item.engin }}
      </v-chip>
    </template> -->
       <template v-slot:[`item.time`]="{ item }">
      <v-chip
        color="black"
        dark
      >
           {{ item.time }}
      </v-chip>
    </template>
    </v-data-table>
  </div>
</template>
<script>
export default {
  created() {
    this.$nuxt.$on("delete", (even) => this.pulldata(even));
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
    this.pulldata();
  },
  methods: {
    // getColorengine(engin){
    //   if(engin=='เครื่องที่ 1')return "blue"
    //    else if(engin=='เครื่องที่ 2')return "orange"
    //    else if(engin=='เครื่องที่ 3')return "#eb4034"
    //           else if(engin=='เครื่องที่ 4')return "#d1944d"
    // },
    getColor(status) {
    if (status == "ขัดข้อง") return "red";
    },
    async pulldata() {
      this.data = await this.$axios.$get("https://tongza.000webhostapp.com/");
      if (this.data.length == 0) this.delbut = false;
      this.$nuxt.$emit("datalen", this.delbut);
      this.data.reverse();
      this.load = false;
    },
  },
};
</script>
