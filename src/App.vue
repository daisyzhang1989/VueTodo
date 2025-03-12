<template>
  <div class="container">
    <div class="nav-bar">
        <router-link to="/" class="nav-link">Home</router-link>
        <router-link to="/about" class="nav-link">About</router-link>
    </div>
    <div class="content">
      <router-view />
    </div>
  </div>
</template>
<script> 
export default {
} 
</script>

<style scoped>

.nav-bar {
  display: flex;
  padding: 10px;
}

.nav-link {
  padding: 8px 12px;
  margin-right: 10px;
  text-decoration: none;
}

</style>

