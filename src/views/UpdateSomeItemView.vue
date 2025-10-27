<template>
  <div class="update-some-item-view-wrapper">
    <h2 class="page-title">Update item</h2>
  </div>
</template>
<script>
import SomeItemForm from '@/components/SomeItemForm/SomeItemForm.vue';
export default {
  name: 'UpdateSomeItemView',
  components: {
    SomeItemForm,
  },
};
</script>
<style>
.update-some-item-view-wrapper {
  padding: 60px;
}
</style>
