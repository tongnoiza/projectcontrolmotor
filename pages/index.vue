<template>
  <div>
    <v-row style="margin-top: 50px;margin-bottom: 100px;" >
      <v-card
      v-for="(i,n) in arr"
      style="margin:15px;width:150px;hight:150px"
      :title="`Slave ${+n+1}`"
    > <v-row>
   <v-col offset="1"> <v-chip class="ma-2" style="top: 6px;background-color: rgba(33,150,243,0.2) !important;" color="primary" variant="text">Slave {{ +n+1 }}</v-chip></v-col>
    </v-row> 
    <v-img src="nano.png" ></v-img> 
          <v-row style="margin-top: -33px">
            <v-col offset="1">
              <v-sheet v-show="i?.lastupdate" class="pa-2 ma-2">  อัพเดตล่าสุด{{ i?.lastupdate }}</v-sheet>
            </v-col>
          </v-row>
        <v-row >
          <v-col>     
        <div v-if="i?.status == 'on'" class="green-light"  ></div>
         <div v-else-if="i?.status == 'off'"  class="red"></div>
         <div v-else></div>
          </v-col>
        </v-row>
  </v-card>
    </v-row>
  </div>
</template>
<script>
export default {
  async fetch() {
    // this.websocket.onmessage = (d) => this.onMessage(d);   
  },
  asyncData(context) {
    return {
      context,
    };
  },

  data() {
    return {
      arr: new Array(32),
      revc: "",
      t: "",
      // websocket: new WebSocket("ws://localhost:3002"),
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
      console.log('sdfsf ',evt);
      let obj = {};
      obj = JSON.parse(evt.data);
    console.log('sdfsf ',obj);
      var currentdate = new Date(); 

  let datetext = currentdate.toTimeString();
datetext = datetext.split(' ')[0];
      obj.lastupdate =" "+ currentdate.getDate() + "/"
                + (currentdate.getMonth()+1)  + "/" 
                + currentdate.getFullYear() + "\n"  
                + datetext
      this.$set(this.arr, obj.id - 1, obj);
    },
    onerror(err){
      // this.websocket.onerror = this
  //     this.websocket= new WebSocket(
  // 'ws://localhost:3001'
  //     )
    },
    onclose(evt){
      // this.websocket.onclose = this
      // this.websocket= new WebSocket(
      //   'ws://localhost:3001'
      // )
    },
    onOpen(evt) {
      // this.websocket.onopen = this;
      // this.sendMessage("Hello world");
    },
    sendMessage(message) {
      // this.websocket.send(this.t);
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
