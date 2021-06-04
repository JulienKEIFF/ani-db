<template>
  <div id="navbar" class="fixed flex flex-row items-center w-full top-0 h-15 bg-blue-400 px-2 shadow-lg z-20">
    <div class="ml-4 text-4xl font-bold text-white"> <a href="/"> ANI-DB </a></div>
    <div id="links" class="w-10/12 flex justify-end text-white gap-4">
      <div class="p-4 text-xl cursor-pointer hover:bg-blue-500 duration-500"> <a href="/"> Home </a></div>
      <div class="p-4 text-xl cursor-pointer hover:bg-blue-500 duration-500">  Animes </div>
      <div class="p-4 text-xl cursor-pointer hover:bg-blue-500 duration-500"> <a href="/users"> Utilisateurs </a></div>
      <div class="p-4 text-xl cursor-pointer hover:bg-blue-500 duration-500"> <a href="/admin">Manage</a></div>
      <div class="p-4 text-xl cursor-pointer hover:bg-blue-500 duration-500"> <a href="/login">Login</a></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "navbar"
}
</script>

<style>

</style>
