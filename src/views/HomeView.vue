<template>
  <div class="home-wrapper">
    <v-form id="home-view-form" class="v-form-wrap" @submit.prevent="onSubmit">
      <some-input :value="someMap" type="checkBtns" label-text="Chechbox buttons" @input="onThisFormCheckInput" />
      <some-input
        ref="someTextarea"
        :value="template"
        type="textarea"
        label-text="Test textarea"
        @input="onThisFormInput($event, 'template')"
      />
      <some-input :value="text" label-text="Test field" @input="onThisFormInput($event, 'text')" />
    </v-form>
    <div class="form-control-box">
      <v-btn :form="'home-view-form'" type="submit" class="v-form-btn">Submit</v-btn>
    </div>
  </div>
</template>
<script>
import SomeInput from '@/components/SomeInput/SomeInput.vue';

export default {
  name: 'Home',
  components: {
    SomeInput,
  },
  data: () => ({
    someMap: [],
    template: '',
    text: '',
  }),
  mounted() {
    this.getTemplate();
    this.getSomeMap();
  },
  methods: {
    getTemplate() {
      this.template = `
      line1:
      line2:
      line3:
      `;
    },
    getSomeMap() {
      this.someMap = { Key1: 'value_1', Key2: 'value_2', Key3: 'value_3' };
    },
    onSubmit() {
      console.log('Submit');
      console.log({ template: this.template, text: this.text });
    },
    onThisFormInput(event, field) {
      if (!field) return;
      this[field] = event;
    },
    onThisFormCheckInput({ value, isChecked }) {
      const textareaEl = this.$refs.someTextarea?.$el.querySelector('textarea');
      if (!textareaEl) return;

      if (!isChecked) {
        this.template = this.template.replace(value, '');
        return;
      }

      const startCursorPosition = textareaEl.selectionStart;
      const endCursorPosition = textareaEl.selectionEnd;

      const beforeText = this.template.slice(0, startCursorPosition);
      const afterText = this.template.slice(endCursorPosition);
      const newTextareaValue = beforeText + value + afterText;

      this.template = newTextareaValue;
    },
  },
};
</script>
<style>
.home-wrapper {
  padding: 60px;
}
.v-form-wrap {
  display: flex;
  flex-direction: column;
  gap: 25px;
  margin-bottom: 25px;
}
.form-control-box {
  display: flex;
  align-items: center;
  gap: 25px;
}
.v-form-btn {
  margin-left: auto;
}
</style>
