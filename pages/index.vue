<template>
<div>
  <v-data-table
  id="data"
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
// console.log('fet api php ',this.datata)
    },
    data () {
      return {datata:[],
        headers: [
          { text: 'สถานะ', value: 'time' },
          { text: 'วัน/เวลา', value: 'engin' },
            { text: 'เครื่องที่', value: 'status' },
         
        ],
      }
    },
    methods: {
    
    },
  }
</script>