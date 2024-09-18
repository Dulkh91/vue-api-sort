<template>
  <div class="container mx-auto w-screen-lg bg-slate-50 p-2">
    <div class="bg-sky-200 p-3 shadow-lg rounded-md mt-2 flex justify-between" 
      v-for="data in datas.value" v-bind:key="data">
      <h1 class="text-blue-600 text-lg font-semibold ml-10">{{data.title}}</h1>
      <p class="mr-10">{{ data.singer }}</p>
    </div>
  </div>
</template>

<script setup>
import {reactive ,onMounted} from 'vue'
import axios from 'axios';

const apiURL = 'https://script.google.com/macros/s/'
const key = 'AKfycbyJ8MadGqqWNk4NTH2zpSdJqxHyVI7kgcLziBKaHCgGtlyjw16vEpDmen3bHhSOQN7y/exec'
const datas = reactive([])


onMounted(async()=>{
  try {
    const getData = await axios(apiURL+key)
    const fbData = []
    getData.data.forEach(d => {
      const baseData = {
        "title": d[0],
        "singer" : d[1],
        "songurl" : d[2]
      }

      //Sort Data (a-z)
      fbData.sort((a,b)=>a.title.localeCompare(b.title))

      fbData.push(baseData)
    });

// តម្រៀបអក្សរខ្មែរមុន
    const khmerRegex = /[\u1780-\u17FF]/
    fbData.sort((a,b)=>{
      if(khmerRegex.test(a.title) && !khmerRegex.test(b.title)){
        return -1
      }else if(!khmerRegex.test(a.title) && khmerRegex.test(b.title)){
        return 1
      }
    })

    //បោះទិន្ន័យចូលទៅ reactive
    datas.value = fbData

  } catch (error) {
    console.log("Err" + error)
  }
 
})

</script>