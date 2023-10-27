<template>
  <div>
    <v-row style="margin-top: 50px;margin-bottom: 100px;" >
      <v-card
      v-for="(i,ii) in arr"
      style="margin:15px;width:150px;hight:150px"
      title="This is a title"
      subtitle="This is a subtitle"
      text="This is content"
    >
    <v-img src="nano.png" ></v-img> 
          <v-row style="margin-top: -33px">
            <v-col offset="1">
              <v-sheet class="pa-2 ma-2">  อัพเดตล่าสุด{{ i?.lastupdate }}</v-sheet>
            </v-col>
          </v-row>
        <v-row >
          <v-col>     
        <div v-if="i?.status == 'on'" class="green-light"  ></div>
         <div v-else  class="red"></div>
          </v-col>
        </v-row>
  </v-card>
  
 
      <!-- <v-col v-for="(i,ii) in arr" cols="1" > 
    <v-img src="nano.png" ></v-img>  
          <v-row>
            <v-col></v-col>
            <v-col><label >อัพเดตล่าสุด{{ i?.lastupdate }}</label></v-col>
            <v-col></v-col>

          </v-row>
        <v-row >
          <v-col>     
        <div v-if="i?.status == 'off'" class="red" ></div>
         <div v-if="i?.status == 'on'" class="green-light" ></div>
          </v-col>
        </v-row>
      </v-col> -->
    </v-row>

    <!-- <v-row>
      <v-col></v-col>
      <v-col cols="8">
        <v-data-table
          id="data"
          :loading="load"
          loading-text="กำลังโหลดข้อมูล..."
          :headers="headers"
          :items="data"
          :items-per-page="5"
          class="elevation-1"
        >
          <template v-slot:[`item.log`]="{ item }">
            <v-chip :color="getColor(item.log)">
              {{ item.log }}
            </v-chip>
          </template>
        </v-data-table>
      </v-col>
      <v-col></v-col>
    </v-row> -->
  </div>
</template>
<script>
import { mapState } from "vuex";
export default {
  created() {
    this.$nuxt.$on("delete", (even) => this.pulldata(even));
  },
  async fetch() {
    this.websocket.onmessage = (d) => this.onMessage(d);
console.log('this.websocket ',this.websocket);
    let o = { id: 2, status: "on" };
    this.arr[o.id-1] = o
    console.log(this.arr);
      
        // setInterval(async () => {
        //   this.data = await this.$axios.$get(
        //   "https://sontaya.000webhostapp.com/getlog.php"
        // );
        // console.log("context");
        // }, 10000);
        // let k = Object.keys(this.data)
    // if (this.data.length == 0) this.delbut = false;
    // this.$nuxt.$emit("datalen", this.delbut);
    this.load = false;
  },
  mounted() {},
  asyncData(context) {
    console.log("context", context);
    return {
      context,
    };
  },

  data() {
    return {
      arr: new Array(32),
      revc: "",
      t: "",
      websocket: new WebSocket(
        "wss://free.blr2.piesocket.com/v3/1?api_key=DzDDGthR65AMeJg4dLv2xhzLrDRGljQew7CuuQ2J&notify_self=1"
      ),
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
  computed: {},
  inject: {
    theme: {
      default: { isDark: false },
    },
  },
  methods: {
    onMessage(evt) {
      let obj = {};
      obj = JSON.parse(evt.data);
      var currentdate = new Date(); 
      obj.lastupdate =" "+ currentdate.getDate() + "/"
                + (currentdate.getMonth()+1)  + "/" 
                + currentdate.getFullYear() + " เวลา"  
                + currentdate.getHours().toLocaleString()+ ":"  
                + currentdate.getMinutes().toLocaleString() + ":" 
                + currentdate.getSeconds().toLocaleString();
      console.log("obj ", obj);
      this.$set(this.arr, obj.id - 1, obj);
    },
    onerror(err){
      this.websocket.onerror = this
      console.log('onerror ',err);
      this.websocket= new WebSocket(
        "wss://free.blr2.piesocket.com/v3/1?api_key=DzDDGthR65AMeJg4dLv2xhzLrDRGljQew7CuuQ2J&notify_self=1"
      )
    },
    onclose(evt){
      this.websocket.onclose = this
      console.log('onclose ',evt.data);
      this.websocket= new WebSocket(
        "wss://free.blr2.piesocket.com/v3/1?api_key=DzDDGthR65AMeJg4dLv2xhzLrDRGljQew7CuuQ2J&notify_self=1"
      )
    },
    onOpen(evt) {
      this.websocket.onopen = this;
      this.sendMessage("Hello world");
    },
    sendMessage(message) {
      this.websocket.send(this.t);
    },
    // async pulldata(even) {
    //   console.log("event ", even);
    //   this.data = await this.$axios.$get(
    //     "https://sontaya.000webhostapp.com/getlog.php"
    //   );
    //   this.data.reverse();
    // },
    // getColor(status) {
    //   if (status.includes("ขัดข้อง")) return "red";
    //   else if (status.includes("เปิด")) return "green";
    //   else if (status.includes("ปิด")) return "black";
    // },
  },
};
</script>
<style>
:root{
  --size:15px
}

.inner {
  position: relative;
  left: 50%;
  top: -15px;
  background-color: transparent;
  width: var(--size);
  height: var(--size);
  border-radius: var(--size);
  box-shadow: 0 0 38px #f62a2a, inset 0 0 8px #f70d67;
  /* -webkit-animation: pulse 2s linear 1s infinite; */
}

.green-led {
    /* align-content: revert; */
    position: relative;
    left: 43%;
    top: -29px;
  background-color: transparent;
  width: var(--size);
  height: var(--size);
  border-radius: var(--size);
  box-shadow: 0px 0px 38px #00ff00, inset 0 0 8px #00ff00;
}

.green-light {
   position: relative;
   left: 43%;
    top: -29px;
  background-color: transparent;
  background-color: #65ff3c;
  width: var(--size);
  height: var(--size);
  border-radius: var(--size);
  box-shadow: 1px 0px 38px 5px #00ff00, inset 0 0 8px #00ff00;
}
.red {
  position: relative;
  left: 43%;
    top: -29px;
  background-color: transparent;
  background-color: #65ff3c;
  width: var(--size);
  height: var(--size);
  border-radius: var(--size);
  box-shadow: 1px 0px 38px 5px #f62a2a, inset 0 0 8px #f70d67;
}
.inner p {
  display: block;
  text-align: center;
  line-height: 25px;
  font-family: sans-serif;
  font-weight: 50;
  font-size: 24px;
  color: #ffc508;
  text-shadow: 0 0 4px #fa881e;
}
</style>
