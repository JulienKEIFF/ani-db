<template>
  <div id="form" class="mx-auto w-1/2 bg-blue-50 p-4 rounded-xl shadow-md">

    <div class="flex flex-row gap-4 mx-auto justify-center">
      <div class="text-2xl border-blue-500 py-1 cursor-pointer" :class="blogin ? 'border-b-2 text-blue-500' : ''"
           id="login" @click="blogin = true">
        Connexion
      </div>
      <div class="text-2xl border-blue-500 py-1 cursor-pointer" :class="!blogin ? 'border-b-2 text-blue-500' : ''"
           id="register"
           @click="blogin = false">S'inscrire
      </div>
    </div>

    <div class="p-10 rounded flex justify-center items-center flex-col" v-if="blogin">

      <div class="flex flex-row items-center mb-5">
        <font-awesome-icon icon="user" class="mr-4 text-3xl text-blue-400"/>
        <input class="p-3 w-80 focus:border-blue-400 rounded border-2 outline-none" type="text"
               name="username"
               autocomplete="off" placeholder="Pseudo" v-model="username">
      </div>

      <div class="flex flex-row items-center mb-5">
        <font-awesome-icon icon="key" class="mr-4  text-3xl text-blue-400"/>
        <input class="p-3 w-80 focus:border-blue-400 rounded border-2 outline-none" type="password" name="password"
               autocomplete="off" placeholder="Mot de passe" v-model="password">
      </div>

      <button class="bg-blue-400 hover:bg-blue-500 text-white font-bold p-2 rounded w-96" @click="login">Connexion
      </button>
    </div>

    <div class="p-10 rounded flex justify-center items-center flex-col" v-if="!blogin">
      <div class="flex flex-row items-center mb-5">
        <font-awesome-icon icon="user" class="mr-4  text-3xl text-blue-400"/>
      <input class="p-3 w-80 focus:border-blue-400 rounded border-2 outline-none" type="text" name="username"
             v-model="username" autocomplete="off" placeholder="Pseudo">
      </div>

      <div class="flex flex-row items-center mb-5">
        <font-awesome-icon icon="envelope" class="mr-4  text-3xl text-blue-400"/>
      <input class="p-3 w-80 focus:border-blue-400 rounded border-2 outline-none" type="text" name="email"
             v-model="email" autocomplete="off" placeholder="Email">
      </div>

      <div class="flex flex-row items-center mb-5">
        <font-awesome-icon icon="key" class="mr-4  text-3xl text-blue-400"/>
      <input class="p-3 w-80 focus:border-blue-400 rounded border-2 outline-none" type="password" name="password"
             v-model="password" autocomplete="off" placeholder="Mot de passe">
      </div>

      <button class="bg-blue-400 hover:bg-blue-500 text-white font-bold p-2 rounded w-96" @click="register">S'inscrire
      </button>
    </div>

    <div v-if="err">
      {{ err }}
    </div>
  </div>
</template>

<script>
import Cookies from 'js-cookie'
import 'js-snackbar/snackbar.css';
import { show, ACTION_TYPE } from 'js-snackbar';
export default {
  name: "login",
  data() {
    return {
      username: '',
      email: '',
      password: '',
      blogin: true,
      err: null
    }
  },
  methods: {
    register: function () {
      this.$axios.post('/auth/local/register', {
        username: this.username,
        email: this.email,
        password: this.password
      })
      .then(res => { this.createCookie(res.data.jwt) })
      .catch(err => show({text: 'Une erreur est survenue. Vérifier votre email', pos: 'top-right', backgroundColor: '#EF4444'}) )
    },

    login: function () {
      this.$axios.post('/auth/local', {
        identifier: this.username,
        password: this.password
      })
      .then(res => { this.createCookie(res.data.jwt) })
      .catch(err => show({text: 'Une erreur est survenue. Vérifier vos identifiants', pos: 'top-right', backgroundColor: '#EF4444'}) )
    },
    createCookie: function (token) {
      Cookies.set('access_token', token);
      console.log(Cookies.get('access_token'));
    }
  }
}
</script>

<style>

</style>
