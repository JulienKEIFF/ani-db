<template>
  <div id="full-item" class="relative w-10/12 bg-blue-50 mx-auto p-4 rounded-xl flex">
    <div class="h-full w-full order-2 ml-4 my-auto">
      <img :src="anime.cover ? apiBase + anime.cover[0].url : ''" class="h-full w-64 object-cover max-h-96 mr-8 ml-auto rounded shadow"/>
    </div>
    <div class="">
      <h1 class="text-4xl mb-4 w-11/12">{{ anime.title }}</h1>
      <div class="min-h-16">
      <span class="bg-blue-400 text-white rounded-xl px-4 py-0.5 mx-1 mb-4 text-sm"
            v-for="(cat, i) in splitJoin(anime.genres)" :key="i">{{ cat }}</span>
        <div class="mt-4">
          <font-awesome-icon icon="tv" class="mr-4"/>
          {{ getType }}
        </div>
        <div class="mt-2 capitalize">
          <font-awesome-icon icon="broadcast-tower" class="mr-4"/>
          {{ anime.status }}
        </div>
        <div class="mt-2">
          <font-awesome-icon icon="pencil-ruler" class="mr-4"/>
          {{ anime.studios }}
        </div>
        <div class="mt-4 text-justify"> {{ anime.description }}</div>
      </div>

      <div class="mt-8">
        <div><span class="font-bold">Nombre d'épisodes:</span> {{ anime.episodes }}</div>
        <div><span class="font-bold">Durée d'un épisode:</span> {{ anime.episodeDuration }} min</div>
        <div><span class="font-bold">Diffusion du</span> {{ formatDate(anime.startDate) }} <span
          class="font-bold">au</span> {{ formatDate(anime.endDate) }}
        </div>
        <div><span class="font-bold">Source de l'anime:</span><span class="capitalize"> {{ anime.source }}</span></div>
        <div><span class="font-bold">Score moyen (source Anilist):</span> {{ anime.score }}%</div>
      </div>
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
    getType: function () {
      switch (this.anime.type) {
        case "tvshow":
          return 'Série TV'
        default:
          return ''
      }
    },
  },
  methods: {
    splitJoin(genres) {
      if (genres !== undefined) return genres.split(', ');
      else return genres
    },
    formatDate: function (date) {
      if (date !== undefined && date !== null) return date.replace(/-/g, '/')
      else return date
    }
  },
  mounted: function () {
    this.$axios.get('anime-shows/' + this.id)
      .then(res => this.anime = res.data)
  }
}
</script>
