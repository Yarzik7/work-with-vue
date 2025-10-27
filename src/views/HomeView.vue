<template>
  <div class="home-wrapper">
    <nav class="home-nav">
      <router-link to="/some-items" class="nav-link">Items</router-link>
      <router-link to="/create-item" class="nav-link">Create</router-link>
      <router-link to="/update-item" class="nav-link">Update</router-link>
    </nav>

    <router-view />
  </div>
</template>
<script>
export default {
  name: 'Home',
};
</script>
<style>
.home-nav {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 20px;
}
.nav-link {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  border: 1px solid #fff;
  border-radius: 8px;
}
</style>
