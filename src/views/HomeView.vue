<template>
  <div class="home-wrapper">
    <v-form id="home-view-form" class="v-form-wrap" @submit.prevent="onSubmit">
      <some-input
        ref="btns"
        :value="someMap"
        type="checkBtns"
        label-text="Chechbox buttons"
        :selected-values="selectedValues"
        @input="onThisFormCheckInput"
      />
      <some-input
        ref="someTextarea"
        :value="template"
        type="textarea"
        label-text="Test textarea"
        @input="onThisFormInput($event, 'template')"
      />
      <some-input
        :value="someType"
        type="select"
        label-text="Select"
        :items="typeItems"
        :item-text="'text'"
        :item-value="'value'"
        @input="onThisFormInput($event, 'someType')"
      />
      <div v-if="someType">
        <some-input
          v-if="someType === 't_1'"
          :value="someOps1"
          type="multi-select"
          label-text="Multi Select 1"
          :items="optionItems1"
          :item-text="'text'"
          :item-value="'value'"
          @input="onThisFormInput($event, 'someOps1')"
        />
        <some-input
          v-else
          :value="someOps2"
          type="multi-select"
          label-text="Multi Select 2"
          :items="optionItems2"
          :item-text="'text'"
          :item-value="'value'"
          @input="onThisFormInput($event, 'someOps2')"
        />
      </div>

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
    typeItems: [
      { text: 'OneT', value: 't_1' },
      { text: 'TwoT', value: 't_2' },
      { text: 'ThreeT', value: 't_3' },
    ],
    optionItems1: [
      { text: 'OneOp1', value: 'op1_1' },
      { text: 'TwoOp1', value: 'op1_2' },
      { text: 'ThreeOp1', value: 'op1_3' },
    ],
    optionItems2: [
      { text: 'OneOp2', value: 'op2_1' },
      { text: 'TwoOp2', value: 'op2_2' },
      { text: 'ThreeOp2', value: 'op2_3' },
    ],
    someMap: [],
    template: '',
    text: '',
    someType: '',
    someOps1: [],
    someOps2: [],
  }),
  computed: {
    selectedValues() {
      return Object.values(this.someMap).filter(v => this.template.includes(v));
    },
  },
  mounted() {
    this.getTemplate();
    this.getSomeMap();
    // this.$refs.btns.setSelVal(Object.values(this.someMap).filter(v => this.template.includes(v)));
    // console.log(Object.values(this.someMap).filter(v => this.template.includes(v)));
  },
  methods: {
    getTemplate() {
      this.template = 'line1: ${value_1}\nline2: ${value_2}\nline3:.';
    },
    getSomeMap() {
      this.someMap = { Key1: '${value_1}', Key2: '${value_2}', Key3: '${value_3}' };
    },
    onSubmit() {
      console.log('Submit');
      console.log({
        template: this.template,
        text: this.text,
        someType: this.someType,
        ops1: this.someOps1,
        ops2: this.someOps2,
      });
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
