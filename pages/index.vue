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
console.log('fet api php ',this.datata)
    },
    data () {
      return {datata:[],
        headers: [
          { text: 'ลำดับ', value: 'a' },
          { text: 'เครื่องที่', value: 'n' },
            { text: 'สถานะ', value: 'p' },
             { text: 'ลำดับ', value: '0' },
          { text: 'เครื่องที่', value: '1' },
            { text: 'สถานะ', value: '2' },
        ],
      }
    },
    methods: {
    
    },
  }
</script>