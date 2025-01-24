<template>
  <div class="about-container">
    <h1>Welcome to about page</h1>
    <nav>
      <NuxtLink to="/" class="nav-link">Home</NuxtLink>
      <a href="https://v2.nuxt.com/" target="_blank" class="nav-link"
        >Nuxt.js V2</a
      >
    </nav>
  </div>
</template>

<style scoped>
.about-container {
  text-align: center;
  margin-top: 50px;
}

nav {
  margin-bottom: 20px;
}

.nav-link {
  margin: 0 10px;
  color: #42b983;
  text-decoration: none;
}

.nav-link:hover {
  text-decoration: underline;
}

h1 {
  font-size: 24px;
  color: #333;
}
</style>
