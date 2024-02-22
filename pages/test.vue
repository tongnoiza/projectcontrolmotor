<template>
    <div>
        <v-text-field v-model="arr.form" label="Label"></v-text-field>
        <v-text-field v-model="arr.form1" @keypress="kp"  label="Label"></v-text-field>
        {{ arr }}
    </div>
</template>
<script setup>
import { ref, watch } from 'vue'
let arr = ref({})
let text = ''


const kp = v => {
    if(!v.key.match(/^[0-9]-?/g)){
        v.preventDefault()
    } else{
        let r = /[\d]{3}\,/g.match(arr.value.form1)
        console.log('gh ',r);
    }
    
}

</script>