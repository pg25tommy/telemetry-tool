<!-- Copyright (C) Tommy Minter 2024 -->
<script setup>
// Importing RouterLink and RouterView components from vue-router
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <nav>
    <div class = "Title">CATATONIC</div>
    <!-- Router links for navigation -->
    <RouterLink to="/home">Home</RouterLink>
    <RouterLink to="/charts">Charts</RouterLink>
    <RouterLink to="/about">About</RouterLink>
  </nav>
  <RouterView />
</template>

<style scoped>
/* Styles for the navigation bar */
nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

/* Styles for the application title */
.Title {
  font-size: 65px;
}

/* Styles for individual navigation links */
nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

/* Removing border for the first navigation link */
nav a:first-of-type {
  border: 0;
}

/* oginal Media query for larger from defualt vue */
@media (min-width: 1024px) {
  /* Adjusting navigation bar style for larger screens */
  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;
    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>