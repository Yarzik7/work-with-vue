<template>
  <div class="create-some-item-view-wrapper">
    <h2 class="page-title">Create item</h2>
    <some-item-form />
  </div>
</template>
<script>
import SomeItemForm from '@/components/SomeItemForm/SomeItemForm.vue';

export default {
  name: 'CreateSomeItemView',
  components: {
    SomeItemForm,
  },
};
</script>
<style>
.create-some-item-view-wrapper {
  padding: 60px;
}
</style>
