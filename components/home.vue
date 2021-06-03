<template>
  <div class="m-auto mt-4 w-11/12 bg-blue-50 rounded-md p-4 mb-16">
    <label for="search" class="mr-4">Rechercher un anime</label>
    <font-awesome-icon icon="search" />
    <input id="search" type="text" class="px-2 w-10/12 rounded-xl bg-blue-200 mb-8" v-model="search">

    <div v-for="anime in filteredItems" :key="anime.id" class="flex flex-row odd:bg-blue-500">
      <img :src="anime.cover[0] ? apiBase + anime.cover[0].url : ''" class="w-36 mr-4 my-4" />
      <div class="flex flex-col relative">
        <div class="font-bold text-xl cursor-pointer"> <a :href="'/anime/'+anime.id"> {{anime.title}} </a> </div>
        <div class="mt-2 text-sm"> {{anime.description}} </div>
        <div class="mt-4 text-sm absolute bottom-3 "> <u>Date de sortie:</u> <span class="capitalize">{{anime.season}} {{getYear(anime.startDate)}}</span> </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      apiBase: apiBase,
      animeList: [],
      selectAnime: [],
      search: '',
    }
  },
  computed: {
    filteredItems() {
      return this.animeList.filter(item => {
        return item.title.toLowerCase().indexOf(this.search.toLowerCase()) > -1
      })
    }
  },
  methods: {
    getYear: function(date) {
      if(date !== undefined && date !== null) return date.split('-')[0]
    }
  },
  mounted: function(){
    this.$axios.get('anime-shows')
    .then(res => { this.animeList = res.data })
  }
}
</script>
