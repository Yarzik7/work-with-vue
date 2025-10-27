<template>
  <div class="some-items-view-wrapper">
    <h2 class="page-title">Some items</h2>
    <ul class="some-items-list">
      <li v-for="value in source" class="some-items-list__item">
        <div class="some-items-list__item-inner-wrap">
          <p class="some-items-list__item-caption">Some item 1</p>
          <div class="some-items-list__item-actions">
            <router-link to="/update-item" class="action-link">Update</router-link>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'SomeItemsView',
};
</script>
<style>
.some-items-view-wrapper {
  padding: 60px;
}
.some-items-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.some-items-list__item-inner-wrap {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  border: 1px solid #fff;
  border-radius: 8px;
}
.action-link {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;

  border: 1px solid #fff;
  border-radius: 8px;
}
.action-link:hover {
  transform: scale(1.05);
}
</style>
