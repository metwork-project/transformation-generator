<template>
  <b-navbar>
    <template #brand>
      <b-navbar-item tag="router-link" :to="{ path: '/' }">
        <img src="~assets/metwork.png" alt="Buefy" height="28" />
      </b-navbar-item>
    </template>
    <template #start>
      <NuxtLink to="/" class="navbar-item">
        Transformation Generator
      </NuxtLink>
      <NuxtLink to="/about" class="navbar-item">
        About
      </NuxtLink>
      <!-- <b-navbar-item href="#">
        Contact
      </b-navbar-item> -->
    </template>
  </b-navbar>
</template>

<script>
export default {
  name: 'About'
}
</script>
