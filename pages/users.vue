<template>
  <div class="m-auto w-11/12 bg-blue-100 rounded-md p-4 mb-8">

    <div id="add-entry-form" class="flex flex-col border-b-2 border-blue-500">
      <label for="name">Nom</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="name" type="text" v-model="newUser.username" />

      <label for="email">E-Mail</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="email" type="text" v-model="newUser.email" />

      <label for="password">Mot de passe</label>
      <input class="mb-5 p-3 focus:border-blue-400 rounded border-2 outline-none" id="password" type="password" v-model="newUser.password" />

      <button id="add-entry-btn" class="mt-8 bg-blue-500 w-64 mx-auto rounded-xl py-2 text-white text-xl mb-8" v-if="!update" @click="addEntry()">Ajouter un utilisateur</button>
      <button id="add-entry-btn" class="mt-8 bg-blue-500 w-64 mx-auto rounded-xl py-2 text-white text-xl mb-8" v-if="update" @click="saveUpdate()">Modifier l'utilisateur</button>
    </div>

    <div class="relative flex flex-row items-center odd:bg-blue-500 my-2 mt-4">
      <div class="relative block font-bold text-xl w-96"> Nom </div>
      <div class="font-bold text-xl w-72"> E-Mail </div>
    </div>

    <div v-for="user in response" :key="user.id" class="relative flex flex-row items-center odd:bg-blue-500 my-2">
      <div class="relative block text-xl w-96"> {{user.username}} </div>
      <div class="text-xl w-72"> {{user.email}} </div>
      <div class="font-bold text-2xl absolute right-0.5 cursor-pointer" @click="removeModal(user.id)">X</div>
      <div class="font-bold text-xl absolute right-6 cursor-pointer" @click="editEntry(user)"><font-awesome-icon icon="pen" class="mr-4" /></div>
    </div>

  <modal :name="'utilisateur'" v-if="modal" @validate="removeEntry" />
  </div>
</template>

<script>
export default {
  name: "user-list",
  data() {
    return{
      response: [],
      newUser: {
        username: '',
        email: '',
        password: '',
      },
      id: null,
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
      this.$axios.delete('users/'+this.removeId)
        .then(_=> { this.updateEntries() })
    },
    addEntry: function() {
      this.$axios.post(`users/`, this.newUser)
        .then(_=>{ this.clearInput() })
    },
    updateEntries: function() {
      this.$axios.get('users/')
        .then(res => { this.response = res.data })
    },
    saveUpdate: function(){
      this.$axios.put('users/'+this.id, this.newUser)
        .then(res => { this.update = false; this.clearInput() })
    },
    editEntry: function(user) {
      this.newUser.username = user.username;
      this.newUser.email = user.email;
      this.newUser.password = user.password;
      this.id = user.id;
      this.update = true;
    },
    clearInput: function(){
      this.newUser.username = '';
      this.newUser.email = '';
      this.newUser.password = '';
      this.id = null;
      this.updateEntries();
    }
  },
  mounted: function(){
    this.updateEntries();
  }
}
</script>

<style>

</style>
