<template>
  <div class="m-auto w-11/12 bg-blue-100 rounded-md p-4 mb-8">
    <button class="bg-blue-400 hover:bg-blue-500 text-white font-bold p-2 rounded w-full" @click="addEntry">Ajouter un utilisateur</button>
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

  <modal :name="'utilisateur'" v-if="modal" @validate="removeEntry" @cancel="modal = false" />
  <user-modal :content="newUser" :type="edit ? 'Modifier' : 'Ajouter'" v-if="userModal" @close="userModal = false; edit = false" @updated="clearInput" />

  </div>
</template>

<script>
export default {
  name: "user-list",
  data() {
    return{
      response: [],
      newUser: {
        id: '',
        username: '',
        email: '',
        password: '',
      },
      id: null,
      removeId: null,
      update: false,
      modal: false,
      userModal: false,
      edit: false,
    }
  },
  methods: {
    removeModal: function(id) {
      this.removeId = id;
      this.modal = true;
    },

    removeEntry: function() {
      this.modal = false
      this.$axios.delete('users/'+this.removeId)
        .then(_=> { this.reloadEntries() })
    },

    addEntry: function(){ this.userModal = true; },

    reloadEntries: function() {
      this.$axios.get('users/')
        .then(res => { this.response = res.data })
    },

    editEntry: function(user) {
      this.newUser.id = user.id;
      this.newUser.username = user.username;
      this.newUser.email = user.email;
      this.newUser.password = user.password;
      this.id = user.id;
      this.update = true;
      this.userModal = true;
      this.edit = true;
    },

    clearInput: function(){
      this.userModal = false;
      this.edit = false;
      this.newUser.id = '';
      this.newUser.username = '';
      this.newUser.email = '';
      this.newUser.password = '';
      this.id = null;
      this.reloadEntries();
    }
  },
  mounted: function(){
    this.reloadEntries();
  }
}
</script>

<style>

</style>
