<template>
  <div id="add-entry-form" class="absolute top-0 left-32 w-10/12 bg-red-500 flex flex-col p-8 mt-8 mb-4 z-50">
      <label for="title">Titre</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="title" type="text" v-model="content.title" />

      <label for="descr">Description</label>
      <textarea class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="descr" type="text" v-model="content.description" />

      <label for="season">Saison</label>
      <select class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none bg-white" id="season" v-model="content.season">
        <option value="printemps">Printemps</option>
        <option value="ete">Eté</option>
        <option value="automne">Automne</option>
        <option value="hiver">Hiver</option>
      </select>

      <label for="status">Status</label>
      <select class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none bg-white" id="status" v-model="content.status">
        <option value="termine">Terminé</option>
        <option value="encours">En cours</option>
        <option value="pasencoresorti">Pas encore sorti</option>
        <option value="annule">Annulé</option>
      </select>

      <label for="episode">Nombre d'épisodes</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="episode" type="number" v-model="content.episodes" />

      <label for="episode-duration">Durée d'un épisode</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="episode-duration" type="number" v-model="content.episodeDuration" />

      <label for="score">Score</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="score" type="number" v-model="content.score" />

      <label for="studio">Studio d'animation</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="studio" type="text" v-model="content.studios" />

      <label for="genres">Genres (séparée par une virgule)</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="genres" type="text" v-model="content.genres" />

      <label for="source">Source</label>
      <select class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none bg-white" id="source" v-model="content.source">
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
</template>

<script>
export default {
  name: "anime-modal",
  props: ['content'],
  methods: {
    
  }
}
</script>

<style>

</style>