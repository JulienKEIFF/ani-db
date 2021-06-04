<template>
  <div id="modal">
    <div id="bg" class="w-full h-full bg-black opacity-50 absolute top-0 left-0 z-30" @click="$emit('close')"></div>

    <div class="absolute top-0 left-32 w-10/12 bg-gray-100 flex flex-col p-8 mt-8 mb-4 z-50 rounded-2xl" >
      <div id="add-entry-form" class="grid grid-cols-5 items-center">
        <label for="title" class="col-span-1 mb-5">Titre</label>
        <input 
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none col-span-4"
          id="title"
          type="text"
          v-model="content.title"
        />

        <label for="descr" class="col-span-1 mb-5">Description</label>
        <textarea
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none col-span-4"
          id="descr"
          type="text"
          v-model="content.description"
        />

        <label for="season" class="col-span-1 mb-5">Saison</label>
        <select
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none bg-white col-span-4"
          id="season"
          v-model="content.season"
        >
          <option value="printemps">Printemps</option>
          <option value="ete">Eté</option>
          <option value="automne">Automne</option>
          <option value="hiver">Hiver</option>
        </select>

        <label for="status" class="col-span-1 mb-5">Status</label>
        <select
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none bg-white col-span-4"
          id="status"
          v-model="content.status"
        >
          <option value="termine">Terminé</option>
          <option value="encours">En cours</option>
          <option value="pasencoresorti">Pas encore sorti</option>
          <option value="annule">Annulé</option>
        </select>

        <label for="episode" class="col-span-1 mb-5">Nombre d'épisodes</label>
        <input
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none col-span-4"
          id="episode"
          type="number"
          v-model="content.episodes"
        />

        <label for="episode-duration" class="col-span-1 mb-5"
          >Durée d'un épisode</label
        >
        <input
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none col-span-4"
          id="episode-duration"
          type="number"
          v-model="content.episodeDuration"
        />

        <label for="score" class="col-span-1 mb-5">Score</label>
        <input
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none col-span-4"
          id="score"
          type="number"
          v-model="content.score"
        />

        <label for="studio" class="col-span-1 mb-5">Studio d'animation</label>
        <input
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none col-span-4"
          id="studio"
          type="text"
          v-model="content.studios"
        />

        <label for="genres" class="col-span-1 mb-5"
          >Genres (séparée par une virgule)</label
        >
        <input
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none col-span-4"
          id="genres"
          type="text"
          v-model="content.genres"
        />

        <label for="source" class="col-span-1 mb-5">Source</label>
        <select
          class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none bg-white col-span-4"
          id="source"
          v-model="content.source"
        >
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

        <label for="img" class="col-span-1 mb-5">Couverture</label>
        <input
          class="mb-5 p-3 focus:border-blue-400 rounded outline-none col-span-4"
          type="file"
          accept="image/*"
          id="img"
        />
      </div>
      <div class="p-3 mt-2 text-center space-x-4 md:block">
        <button
          id="add-entry-btn"
          class="mx-auto bg-blue-400 hover:bg-blue-500 text-white font-bold p-2 rounded w-80"
          @click="$emit('close')"
        >
          Annuler
        </button>
        <button
          id="add-entry-btn"
          class="mx-auto bg-green-400 hover:bg-green-500 text-white font-bold p-2 rounded w-80"
          @click="type == 'Modifier' ? saveUpdate() : newEntry()"
        >
          {{ type }} l'anime
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "anime-modal",
  props: ["content", "type"],
  methods: {
    addEntry: function () {
      this.$axios.post("anime-shows", this.newAnime);
      this.updateEntries();
    },

    saveUpdate: function(){
      console.log(this.content)
      this.$axios.put('anime-shows/'+this.content.id, this.content)
        .then(res => { this.$emit('updated') })
    },

    newEntry: function() {
      this.$axios.post(`anime-shows/`, this.content)
        .then(_=>{ this.$emit('updated') })
    },
  },
};
</script>

<style>
</style>
