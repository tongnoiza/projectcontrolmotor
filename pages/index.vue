<template>
  <div>
    <v-row style="margin-top: 50px; margin-bottom: 100px">

      <v-card v-for="(i, n) in arr" :key="n" style="margin: 15px; width: 160px;" :title="`Slave ${+n + 1}`">
        <v-row>
          <v-col>
            <v-chip class="ma-2" style="
                top: 6px;
                background-color: rgba(33, 150, 243, 0.2) !important;
              " color="primary" variant="outlined">Slave {{ +n + 1 }}
            </v-chip>
            <v-chip variant="outlined" style="top: 6px;" v-if="i?.connect == 1" color="green"> online </v-chip>
            <v-chip variant="outlined" style="top: 6px;" v-else color="red"> offline </v-chip>

          </v-col>
        </v-row>
        <v-img src="nano.png"></v-img>
        <v-row style="margin-top: -33px">
          <v-col offset="1">
            <v-sheet v-if="i?.lastupdate" class="pa-2 ma-2">
              อัพเดตล่าสุด{{ i?.lastupdate }}
            </v-sheet>
            <v-sheet v-if="i?.status == 'err'" class="pa-2 ma-2">
              มีข้อผิดพลาด
            </v-sheet>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <div v-if="i?.status == 'on'" class="green-light"></div>
            <div v-else-if="i?.status == 'off' || i?.status == 'err'" class="red-error pa-2 ma-2"></div>
            <!-- <div> มีข้อผิดพลาด</div> -->
          </v-col>
        </v-row>
      </v-card>
    </v-row>
  </div>
</template>
<script>
// import { useWebSocket } from '@vueuse/core'

export default {
  created() {

    // console.log('this.websocket. ', this.websocket);

    for (let i = 0; i < 32; i++) {
      this.arr[i] = {};
    }
    // console.log('local data create ',JSON.parse(localStorage.getItem('data')) );
// if(localStorage.getItem('data')){
//   JSON.parse(localStorage.getItem('data')).forEach((v,i) => {
//     console.log('for ',v);
//     this.$set(this.arr, i,  v );
//   });
// }
  },
  mounted(){
    this.websocket.onmessage =  this.onMessage;   
    this.websocket.onerror = v => {
      console.log('err ', v);
    }
    this.websocket.onclose = v => {
      console.log('onclose ', v);
    }
  },
  data() {
    return {
      update: {},
      arr: [],
      websocket: new WebSocket('wss://motorsocket.onrender.com'),
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
  updated(){
    // this.onMessage()
  },
  methods: {
    async onMessage(evt) {
      const rep = evt.data.replace('0','')
      console.log(rep);

 
      this.$forceUpdate()
     await this.$nextTick(async ()=>{
      if (evt.data.charAt(0) != 'o') {
        let obj = {};
        obj = JSON.parse(rep);
        console.log(obj);
        var currentdate = new Date();
        let datetext = currentdate.toTimeString();
        datetext = datetext.split(" ")[0];
        obj.lastupdate =
          " " +
          currentdate.getDate() +
          "/" +
          (currentdate.getMonth() + 1) +
          "/" +
          currentdate.getFullYear() +
          "\n" +
          datetext;

        let update = { ...JSON.parse(JSON.stringify(this.arr[obj.id - 1])), ...obj };
        console.log("update  ", update);
        this.$set(this.arr, obj.id - 1, update);
      } else {
        let t = evt.data.substring(1)
        console.log(t);
        for (let i = 0; i < t.length; i++) {
          let obj = {connect:t.charAt(i)}
    // console.log({obj});
       await this.$set(this.arr, i, { ...JSON.parse(JSON.stringify(this.arr[i])), ...obj });
        }
        // localStorage.setItem('data',JSON.stringify(this.arr))
        // console.log(this.arr)
      }
     })
    },
  },
};
</script>
<style>
:root {
  --size: 15px;
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

.red-error {
  position: relative;
  left: 43%;
  top: -29px;
  background-color: transparent;
  background-color: #f62a2a;
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
