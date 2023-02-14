<template>
  <footer>
    <p>Copyright &copy; {{ new Date().getFullYear() }}</p>
    <a href="/about">About</a>
  </footer>
</template>

<script>
export default {
  name: "Footer"
}
</script>

<style scoped>
a {
  color: #333;
}

footer {
  margin-top: 30px;
  text-align: center;
}
</style>