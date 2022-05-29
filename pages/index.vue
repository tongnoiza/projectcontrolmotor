<template>
<div>
  <v-data-table
  id="data"
  disable-sort  
  :loading="load"
    loading-text="กำลังโหลดข้อมูล..."
    :headers="headers"
    :items="datata"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>
  </div>
</template>
<script>
  export default {
   async fetch(){
this.datata = await this.$axios.$get('https://tongza.000webhostapp.com/')
this.datata.reverse()
this.load =false
 console.log('fet api php ',this.datata)
    },
    data () {
      return {datata:[],
      load:true,
        headers: [    { text: 'เครื่องที่', value: 'engin' },
          { text: 'สถานะ', value: 'status' },
          { text: 'วัน/เวลา', value: 'time' },
        
         
        ],
      }
    },
    methods: {
    
    },
  }
</script>