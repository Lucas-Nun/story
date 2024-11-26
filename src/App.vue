<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="/">storypage</RouterLink>
        <RouterLink to="/settings">Settings</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style>


</style>
