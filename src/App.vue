<script setup>

</script>

<template>
  <div class="min-h-screen bg-gray-100">
    <div class="container py-6">
      <div class="bg-white p-4  shadow rounded ">

        <div class="flex -mx-4 items-center">
          <div class="flex-1 px-4">
            <select @change="getspecificSura" class="quran-input" name="" id="">
              <option value="">Select Sura</option>
              <option :value="sura.number" v-for="sura in suras" value="">{{ sura.name }} - {{ sura.englishName }}</option>

            </select>
          </div>

          <div class="flex-1 px-4 text-center mb-6">
            <h3 class="font-bold mb-1 text-lg">{{currentSura.englishName}}</h3>
            <p>{{currentSura.name}}</p>
            <p>Ayah -{{currentSura.numberOfAyahs}}</p>
          </div>

          <div class="flex-1 px-4">
            <select class="quran-input" name="" id="">
              <option value="">Select Ayah </option>
              <option v-for="sura in suras" value="">{{ sura.numberOfAyahs }} - {{ sura.englishName }}</option>

            </select>
          </div>
        </div>

        <!-- spinner  -->
        <div v-if="loading" class="flex">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 animate-spin">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12a7.5 7.5 0 0015 0m-15 0a7.5 7.5 0 1115 0m-15 0H3m16.5 0H21m-1.5 0H12m-8.457 3.077l1.41-.513m14.095-5.13l1.41-.513M5.106 17.785l1.15-.964m11.49-9.642l1.149-.964M7.501 19.795l.75-1.3m7.5-12.99l.75-1.3m-6.063 16.658l.26-1.477m2.605-14.772l.26-1.477m0 17.726l-.26-1.477M10.698 4.614l-.26-1.477M16.5 19.794l-.75-1.299M7.5 4.205L12 12m6.894 5.785l-1.149-.964M6.256 7.178l-1.15-.964m15.352 8.864l-1.41-.513M4.954 9.435l-1.41-.514M12.002 12l-3.75 6.495" />
          </svg>
          loadning ...
        </div>



        <div v-else class=" text-4xl">
          <p class="flex items-center mb-6" v-if="currentSura.hasOwnProperty('ayahs')" v-for="ayah in currentSura.ayahs">
            <span
              class="text-xs max-w-min px-[.45rem] py-[.1rem] flex items-center justify-center border-green-700 border-2 rounded-full ml-3 mr-6">{{ ayah.numberInSurah }}
            </span> {{ ayah.text }}
            
            <av-waveform
            :canv-height="0"
            :canv-width="0"
            :src="ayah.audio"
          ></av-waveform>
           
          </p>
          
         
        </div>


      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: 'App',
    data() {
      return {
        suras: [],
        currentSura: [],
        loading:true,
      }
    },
    mounted() {
      axios.get('https://api.alquran.cloud/v1/surah')
        .then(response => {
          this.suras = response.data.data
        })
        this.querypecificSura(1);
    },
    methods:{
      getspecificSura(e){
        this.querypecificSura(e.target.value);
      },
      querypecificSura(suraNumber){
        axios.get('https://api.alquran.cloud/v1/surah/' + suraNumber)
        .then(response => {
          console.log(response.data.data);
          this.loading = true;
          this.currentSura = response.data.data;
          this.loading = false;
        })
      }
    }
  };
</script>


<style scoped>

</style>