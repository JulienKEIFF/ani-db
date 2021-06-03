<template>
  <div class="m-auto mt-4 w-11/12 bg-blue-100 rounded-md p-4 mb-8">
    <div v-for="anime in animeList" :key="anime.id" class="relative flex flex-row items-center odd:bg-blue-500">
      <img :src="anime.cover[0] ? apiBase + anime.cover[0].url : ''" class="w-16 h-16 mr-4 my-2 rounded-full" />
      <div class="font-bold text-xl"> {{anime.title}} </div>
      <div class="font-bold text-2xl absolute right-0.5 cursor-pointer" @click="removeEntry(anime.id)">X</div>
    </div>

    <div id="add-entry-form" class="mt-16 flex flex-col">
      <label for="title">Titre</label>
      <input id="title" type="text" v-model="newAnime.title" />

      <label for="descr">Description</label>
      <input id="descr" type="text" v-model="newAnime.description" />

      <label for="season">Saison</label>
      <select id="season" v-model="newAnime.season">
        <option value="printemps">Printemps</option>
        <option value="ete">Eté</option>
        <option value="automne">Automne</option>
        <option value="hiver">Hiver</option>
      </select>

      <label for="img">Couverture</label>
      <input type="file" accept="image/*" id="img"  />

      <button id="add-entry-btn" class="mt-8" @click="addEntry()">Ajouter un anime</button>
    </div>
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
        season: '',
      }
    }
  },
  methods: {
    removeEntry: function(id) {
      this.$axios.delete('anime-shows/'+id)
        .then(_=> { this.updateEntries() })
    },
    addEntry: function() {
      this.$axios.post('anime-shows', this.newAnime)
      this.updateEntries()
    },
    updateEntries: function() {
      this.$axios.get('anime-shows')
        .then(res => { this.animeList = res.data })
    }
  },
  mounted: function(){
    this.updateEntries();
  }
}
</script>

