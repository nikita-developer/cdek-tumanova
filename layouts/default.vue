<template>
  <div class="page">
    <header class="header">
      <NavBar></NavBar>
    </header>
    <main class="main">
      <div class="container">
        <nuxt></nuxt>
      </div>
    </main>
    <footer class="footer">
      <Footer></Footer>
    </footer>
  </div>
</template>

<script>
export default {
  name: "default"
}
</script>

<style lang="scss">
  .page {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }

  .main {
    flex-grow: 1;
  }
</style>
