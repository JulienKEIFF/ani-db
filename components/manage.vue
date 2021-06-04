<template>
  <div class="m-auto mt-4 w-11/12 bg-blue-100 rounded-md p-4 mb-8">
    <div id="add-entry-form" class="mt flex flex-col border-b-2 border-blue-300 pb-8 mb-4">
      <label for="title">Titre</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="title" type="text" v-model="newAnime.title" />

      <label for="descr">Description</label>
      <textarea class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="descr" type="text" v-model="newAnime.description" />

      <label for="season">Saison</label>
      <select class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none bg-white" id="season" v-model="newAnime.season">
        <option value="printemps">Printemps</option>
        <option value="ete">Eté</option>
        <option value="automne">Automne</option>
        <option value="hiver">Hiver</option>
      </select>

      <label for="status">Status</label>
      <select class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none bg-white" id="status" v-model="newAnime.status">
        <option value="termine">Terminé</option>
        <option value="encours">En cours</option>
        <option value="pasencoresorti">Pas encore sorti</option>
        <option value="annule">Annulé</option>
      </select>

      <label for="episode">Nombre d'épisodes</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="episode" type="number" v-model="newAnime.episodes" />

      <label for="episode-duration">Durée d'un épisode</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="episode-duration" type="number" v-model="newAnime.episodeDuration" />

      <label for="score">Score</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="score" type="number" v-model="newAnime.score" />

      <label for="studio">Studio d'animation</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="studio" type="text" v-model="newAnime.studios" />

      <label for="genres">Genres (séparée par une virgule)</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="genres" type="text" v-model="newAnime.genres" />

      <label for="source">Source</label>
      <select class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none bg-white" id="source" v-model="newAnime.source">
        <option value="manga">Manga</option>
        <option value="original">Original</option>
        <option value="lightnovel">Light Novel</option>
        <option value="visualnovel">Visual Novel</option>
        <option value="novel">Novel</option>
        <option value="anime">Anime</option>
        <option value="videogame">Jeu Vidéo</option>
        <option value="doujinshi">Doujinshi</option>
        <option value="other">Autre</option>
      </select>

      <label for="img">Couverture</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded outline-none" type="file" accept="image/*" id="img"  />

      <button v-if="!update" id="add-entry-btn" class="mx-auto bg-blue-400 hover:bg-blue-500 text-white font-bold p-2 rounded w-80" @click="addEntry()">Ajouter un anime</button>
      <button v-if="update" id="add-entry-btn" class="mx-auto bg-blue-400 hover:bg-blue-500 text-white font-bold p-2 rounded w-80" @click="applyUpdate()">Modifier l'anime</button>
    </div>


    <div v-for="anime in animeList" :key="anime.id" class="relative flex flex-row items-center odd:bg-blue-500">
      <img :src="anime.cover[0] ? apiBase + anime.cover[0].url : ''" class="w-16 h-16 mr-4 my-2 rounded-full" />
      <div class="font-bold text-xl"> {{anime.title}} </div>
      <div class="font-bold text-2xl absolute right-0.5 cursor-pointer" @click="removeModal(anime.id)">X</div>
      <div class="font-bold text absolute right-4 cursor-pointer" @click="editEntry(anime)"><font-awesome-icon icon="pen" class="mr-4" /></div>
    </div>

    <modal :name="'utilisateur'" v-if="modal" @validate="removeEntry" @cancel="modal = false" />

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
      update: false,
      modal: false,
    }
  },
  methods: {
    removeModal: function(id) {
      this.removeId = id;
      this.modal = true;
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
    updateEntries: function() {
      this.$axios.get('anime-shows')
        .then(res => { this.animeList = res.data })
    },
    editEntry: function(anime) {
      this.update = true;
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

