<template>
  <div class="m-auto mt-4 w-11/12 bg-blue-100 rounded-md p-4 mb-8">

    <div v-for="anime in animeList" :key="anime.id" class="relative flex flex-row items-center odd:bg-blue-500">
      <img :src="anime.cover[0] ? apiBase + anime.cover[0].url : ''" class="w-16 h-16 mr-4 my-2 rounded-full" />
      <div class="font-bold text-xl"> {{anime.title}} </div>
      <div class="font-bold text-2xl absolute right-0.5 cursor-pointer" @click="removeModal(anime.id)">X</div>
      <div class="font-bold text absolute right-4 cursor-pointer" @click="editEntry(anime)"><font-awesome-icon icon="pen" class="mr-4" /></div>
    </div>

    <modal :name="'utilisateur'" v-if="modal" @validate="removeEntry" @cancel="modal = false" />
    <anime-modal v-if="animeModal" :content="newAnime" />

  </div>
</template>

<script>
export default {
  name: "manage",
  data() {
    return {
      apiBase: apiBase,
      animeList: [],
      newAnime: {
        title: '',
        description: '',
        status: '',
        season: '',
        episodes: 12,
        episodeDuration: 24,
        score: 0,
        studios: '',
        genres: '',
        source: '',
      },
      idUpdate: null,
      removeId: null,
      modal: false,
      animeModal: false,
    }
  },
  methods: {
    removeModal: function(id) {
      this.removeId = id;
      this.modal = true;
    },
    updateEntries: function() {
      this.$axios.get('anime-shows')
      .then(res => { this.animeList = res.data })
    },
    removeEntry: function() {
      this.modal = false
      this.$axios.delete('anime-shows/'+this.removeId)
        .then(_=> { this.updateEntries() })
    },
    addEntry: function() {
      this.$axios.post('anime-shows', this.newAnime)
      this.updateEntries()
    },
    editEntry: function(anime) {
      this.idUpdate = anime.id;
      this.newAnime.title = anime.title;
      this.newAnime.description = anime.description;
      this.newAnime.status = anime.status;
      this.newAnime.season = anime.season;
      this.newAnime.episodes = anime.episodes;
      this.newAnime.episodeDuration = anime.episodeDuration;
      this.newAnime.score = anime.score;
      this.newAnime.studios = anime.studios;
      this.newAnime.genres = anime.genres;
      this.newAnime.source = anime.source;
      this.animeModal = true;

    },
    applyUpdate: function() {
      this.update = false;
      this.idUpdate = null;
      this.newAnime.title = null;
      this.newAnime.description = null;
      this.newAnime.status = null;
      this.newAnime.season = null;
      this.newAnime.episodes = null;
      this.newAnime.episodeDuration = null;
      this.newAnime.score = null;
      this.newAnime.studios = null;
      this.newAnime.genres = null;
      this.newAnime.source = null;
    }
  },
  mounted: function(){
    this.updateEntries();
  }
}
</script>

