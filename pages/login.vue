<template>
  <div id="form" class="mx-auto w-10/12 bg-blue-50 p-4 rounded-xl">
    
    <div class="flex flex-row gap-4 mx-auto justify-center">
        <div class="text-2xl border-blue-500 py-1" :class="blogin ? 'border-b-8' : ''" id="login" @click="blogin = true">Login</div>
        <div class="text-2xl border-blue-500 py-1" :class="!blogin ? 'border-b-8' : ''" id="register" @click="blogin = false">Register</div>
    </div>

    <div class="flex flex-col mt-4" v-if="blogin">
        <label class="" for="Username">Username</label>
        <input class="bg-blue-400 rounded-xl px-2 py-0.5 text-white" type="text" name="username" v-model="username">

        <label class="mt-2" for="Username">Password</label>
        <input class="bg-blue-400 rounded-xl px-2 py-0.5 text-white" type="password" name="password" v-model="password">

        <button class="mt-4 bg-blue-400 rounded-xl w-32 mx-auto py-0.5 text-white" @click="login">Login</button>
    </div>

    <div class="flex flex-col mt-4" v-if="!blogin">
        <label class="" for="username">Username</label>
        <input class="bg-blue-400 rounded-xl px-2 py-0.5 text-white" type="text" name="username" v-model="username">

        <label class="mt-2" for="email">E-Mail</label>
        <input class="bg-blue-400 rounded-xl px-2 py-0.5 text-white" type="text" name="email" v-model="email">

        <label class="mt-2" for="password">Password</label>
        <input class="bg-blue-400 rounded-xl px-2 py-0.5 text-white" type="password" name="password" v-model="password">

        <button class="mt-4 bg-blue-400 rounded-xl w-32 mx-auto py-0.5 text-white" @click="register">Register</button>
    </div>

    <div v-if="err">
        {{err}}
    </div>
  </div>
</template>

<script>
import Cookies from 'js-cookie'
export default {
    name: "login",
    data(){
        return {
            username: '',
            email: '',
            password: '',
            blogin: true,
            err: null
        }
    },
    methods: {
        register: function() {
            this.$axios.post('/auth/local/register', {
                username: this.username,
                email: this.email,
                password: this.password
            })
            .then(res=>{ this.createCookie(res.data.jwt) })
        },

        login: function() {
            this.$axios.post('/auth/local', {
                identifier: this.username,
                password: this.password
            })
            .then(res => { this.createCookie(res.data.jwt) })
        },
        createCookie: function(token) {
            Cookies.set('access_token',token);
            console.log(Cookies.get('access_token'));
        }
    }
}
</script>

<style>

</style>