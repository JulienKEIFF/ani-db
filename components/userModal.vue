<template>
  <div id="user-modal">
    <div id="bg" class="w-full h-full bg-black opacity-50 absolute top-0 left-0 z-30" @click="$emit('close')"></div>

    <div class="absolute top-0 left-32 w-10/12 bg-gray-100 flex flex-col p-8 mt-8 mb-4 z-50 rounded-2xl" >
        <div id="add-entry-form" class="grid grid-cols-5 items-center">
            <label class="col-span-1 mb-5" for="name">Nom</label>
            <input class="col-span-4 mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="name" type="text" v-model="content.username" />

            <label class="col-span-1 mb-5" for="email">E-Mail</label>
            <input class="col-span-4 mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="email" type="text" v-model="content.email" />

            <label class="col-span-1 mb-5" for="password">Mot de passe</label>
            <input class="col-span-4 mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="password" type="password" v-model="content.password" />

            
        </div>

        <div class="p-3 mt-2 text-center space-x-4 md:block">
            <button id="add-entry-btn" 
                class="mx-auto bg-blue-400 hover:bg-blue-500 text-white font-bold p-2 rounded w-80"
                @click="$emit('close')"
            >
                Annuler
            </button>

            <button id="add-entry-btn"
                class="mx-auto bg-green-400 hover:bg-green-500 text-white font-bold p-2 rounded w-80"
                @click="type == 'Modifier' ? saveUpdate() : newEntry()"
            >
                {{ type }} l'utilisateur
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

    saveUpdate: function () {
      this.$axios.put("users/" + this.content.id, this.content)
        .then((res) => { this.$emit("updated") });
    },

    newEntry: function () {
      this.$axios.post(`users/`, this.content)
      .then(_ => { this.$emit("updated") });
    },
  },
};
</script>

<style>
</style>
