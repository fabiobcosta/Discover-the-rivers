<template>
  <article :class="{'dark': toggleActive}">

    <span v-if="$fetchState.pending" class="loading dark:text-white dark:text-opacity-50">Fetching rivers...</span>
    <span v-else-if="$fetchState.error" class="error dark:text-white dark:text-opacity-50">An error occurred :(</span>
    <div v-else>
      <ToggleButton v-on:change="triggerToggleEvent"/>
      <h1 class="text-center title text-green-500 text-4xl font-bold p-4 dark:text-white dark:text-opacity-50">Rivers of the World</h1>
      <button @click="$fetch" class="submit mb-8 p-3 rounded text-indigo-400 bg-purple-100	font-bold w-full dark:text-white dark:text-opacity-50 dark:bg-gray-700">Discover the rivers</button>
      <div v-if="rivers.length === 0">
        <hooper>
          <slide class="border-green-300 border-2 rounded-lg dark:border-purple-900">
            <h1 class="text-center river-title text-gray-500 text-3xl font-bold p-4 bg-green-100 dark:text-white dark:text-opacity-50 dark:bg-gray-600">The most amazing rivers in the world</h1>
            <div class="text-center river-photo-container dark:river-photo-container-dark">
              <img src="../static/default-photo.jpg" class="min-w-full max-w-full" />
              <div class="text-center river-sub-title text-gray-300 text-1xl font-bold p-4 bg-indigo-400 dark:text-white dark:text-opacity-50">Let the whaters flow</div>
            </div>
            <h2 class="text-center river-description text-gray-500 text-base font-bold p-4 m-2 dark:text-white dark:text-opacity-50">For those ho loves the outdoors and nature</h2>
          </slide>
          <hooper-navigation slot="hooper-addons"></hooper-navigation>
        </hooper>
      </div>
      <div v-else>
        <hooper>
          <slide v-for="river in rivers" :key="river.title" :index="river.title" class="border-green-300 border-2 rounded-lg dark:border-purple-900">
            <h1 class="text-center river-title text-gray-500 text-3xl font-bold p-4 bg-green-100 dark:text-white dark:text-opacity-50 dark:bg-gray-600">{{ river.title }}</h1>
            <div class="text-center river-photo-container dark:river-photo-container-dark">
              <img v-bind:src="river.image" v-bind:alt="river.slug" class="min-w-full max-w-full" />
              <div class="text-center river-sub-title text-gray-300 text-1xl font-bold p-4 bg-indigo-400 dark:text-white dark:text-opacity-50">Length: {{ river.length }} ! <span class="p-1">Continent: {{ river.continent }}</span> ! <span class="p-1"> Countries:{{ river.countries }}</span></div>
            </div>
            <h2 class="text-justify river-description text-gray-500 text-base font-bold p-4 m-2 dark:text-white dark:text-opacity-50">{{ river.description }}</h2>
          </slide>
          <hooper-navigation slot="hooper-addons"></hooper-navigation>
        </hooper>

      </div>

    </div>

  </article>
</template>

<script>
  import {
    Hooper,
    Slide,
    Navigation as HooperNavigation
  } from 'hooper';

  import 'hooper/dist/hooper.css';
  import ToggleButton from '../components/ToggleButton.vue'

  export default {
    
    components: {
      Hooper,
      Slide,
      HooperNavigation,
      ToggleButton
    },
    
    mounted() {
       this.$nextTick(() => {
        this.$nuxt.$loading.start()
        setTimeout(() => this.$nuxt.$loading.finish(), 500)
      })
      this.rivers = []
    },

    data() {
      return {
        rivers: [],
        toggleActive: false 
      }
    },

    async fetch() {
      
      //empty = false
      this.rivers = await fetch(`https://api.nuxtjs.dev/rivers`)
        .then((res) => {
          return res.json()
        }).then((data) => {
           return (data.sort((a, b) => parseFloat(a.length.replace("km", "").trim()) - parseFloat(b.length.replace("km", "").replace(",", ".").trim())))
        }).catch((res) => {
          console.error(res)
        })
        
        
    },
    fetchOnServer: true,

    methods: {
        triggerToggleEvent(value) {
            this.toggleActive = value;
        }
    }
  }
 
</script>

<style>

article.dark{
  --tw-bg-opacity: 1;
  background-color: rgba(124, 58, 237, var(--tw-bg-opacity));
}

article > div{
  text-align:center;
   width:70%;
   margin:0 auto;
}

button.submit{

}

.hooper {
    position: relative;
    box-sizing: border-box;
    width: 90%;
    height: auto;
    text-align:center;
    margin: 0 auto;
}
.hooper-next, .hooper-prev {
    background-color: transparent;
    border: none;
    padding: 5px;
    position: absolute;
    top: 30%;
    transform: translateY(-50%);
    cursor: pointer;
    background-color: whitesmoke;
}
</style>
