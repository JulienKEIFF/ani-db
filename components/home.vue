<template>
  <div class="m-auto mt-4 w-11/12 bg-blue-50 rounded-md p-4 mb-16">
    <div class="flex w-1/2 mx-auto items-center">
      <font-awesome-icon icon="search"/>
<!--      <input id="search" type="text" class="px-2 w-full rounded-xl bg-blue-200 ml-2" v-model="search">-->
      <input id="search" type="text" class="ml-2 w-full px-2 focus:border-blue-400 rounded-xl border-2 outline-none" v-model="search">
    </div>

    <div v-for="anime in filteredItems" :key="anime.id"
         class="flex flex-row odd:bg-blue-500 bg-white p-2 m-4 shadow rounded-lg h-64">
      <img :src="anime.cover[0] ? apiBase + anime.cover[0].url : ''" class="w-40 h-60 mr-4 rounded cursor-pointer"
           @click="location.href='/anime/'+anime.id
"/>
      <div class="flex flex-col relative">
        <div class="font-bold text-xl cursor-pointer"><a :href="'/anime/'+anime.id"> {{ anime.title }} </a></div>
        <div class="mt-2 h-40 text-sm overflow-auto"> {{ anime.description }}</div>
        <div class="absolute flex bottom-0 gap-2">
          <div class="bg-blue-400 text-white rounded-xl px-4 py-1 mt-4 text-sm bottom-3 relative font-medium">Date de
            sortie: <span
              class="capitalize">{{ anime.season }} {{ getYear(anime.startDate) }}</span></div>
          <div class="bg-blue-400 text-white rounded-xl px-4 py-1 mt-4 text-sm bottom-3 relative font-medium">Status:
            <span
              class="capitalize">{{ anime.status }}</span></div>
          <div class="bg-blue-400 text-white rounded-xl px-4 py-1 mt-4 text-sm bottom-3 relative font-medium">Score:
            <span
              class="capitalize">{{ anime.score }}%</span></div>
        </div>
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
      location: location,
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
    getYear: function (date) {
      if (date !== undefined && date !== null) return date.split('-')[0]
    }
  },
  mounted: function () {
    this.$axios.get('anime-shows')
      .then(res => {
        this.animeList = res.data
      })
  }
}
</script>
