<template>
  <div id="full-item" class="relative w-10/12 bg-blue-50 mx-auto p-4 rounded-xl">
    <h1 class="text-4xl mb-4 w-8/12">{{anime.title}}</h1>
    <img :src="anime.cover ? apiBase + anime.cover[0].url : ''" class="absolute top-4 right-4 w-3/12" />
    <div class="w-8/12 min-h-16">
      <span class="bg-blue-400 text-white rounded-xl px-4 py-0.5 mx-1 mb-4 text-sm" v-for="(cat, i) in splitJoin(anime.genres)" :key="i">{{cat}}</span>
      <div class="mt-4"> <font-awesome-icon icon="tv" class="mr-4" /> {{getType}}</div>
      <div class="mt-2 capitalize"> <font-awesome-icon icon="broadcast-tower" class="mr-4" /> {{anime.status}}</div>
      <div class="mt-2"> <font-awesome-icon icon="pencil-ruler" class="mr-4" /> {{anime.studios}}</div>
      <div class="mt-4 text-justify"> {{anime.description}} </div>
    </div>

    <div class="mt-8">
      <h1 class="mb-2">Plus d'informations:</h1>
      <div> <span class="font-bold">Nombre d'épisode:</span> {{anime.episodes}}</div>
      <div> <span class="font-bold">Durée d'épisode:</span> {{anime.episodeDuration}} min</div>
      <div> <span class="font-bold">Diffusion du</span> {{formatDate(anime.startDate)}} <span class="font-bold">au</span> {{formatDate(anime.endDate)}} </div>
      <div> <span class="font-bold">Source de l'anime:</span> {{anime.source}}</div>
      <div> <span class="font-bold">Score moyen (source Anilist):</span> {{anime.score}}/100</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "anime",
  props: ['id'],
  data() {
    return {
      apiBase: apiBase,
      anime: {},
    }
  },
  computed: {
    getType: function() {
      switch (this.anime.type) {
        case "tvshow":
          return 'Série TV'
        default:
          return ''
      }
    },
  },
  methods: {
    splitJoin(genres){
      if(genres !== undefined) return genres.split(', ');
      else return genres
    },
    formatDate: function(date) {
      if(date !== undefined && date !== null) return date.replace(/-/g, '/')
      else return date
    }
  },
  mounted: function() {
    this.$axios.get('anime-shows/' + this.id)
    .then(res => this.anime = res.data)
  }
}
</script>
