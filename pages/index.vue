<template>
  <div>
    <NavBar />
    <div class="container text-center">
      <div class="inner">
        <h2 class="font-weight-light">Welcome to Book Management</h2>
        <p v-if="$auth.loggedIn">
          User: {{ $auth.user.name }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  auth: false,
}
</script>

<style scoped>
h2 {
  margin-top: 20%;
}
</style>
