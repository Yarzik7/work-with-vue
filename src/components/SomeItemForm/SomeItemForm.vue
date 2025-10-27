<template>
  <div class="some-item-form-wrapper">
    <v-form id="home-view-form" class="v-form-wrap" @submit.prevent="onSubmit">
      <some-input :value="dataFromFields.name" label-text="Name" @input="onThisFormInput($event, 'text')" />

      <some-input
        ref="someTextarea"
        :value="dataFromFields.description"
        type="textarea"
        label-text="Description"
        @input="onThisFormInput($event, 'description')"
      />

      <some-input
        :value="dataFromFields.type"
        :items="itemTypes"
        :item-text="'text'"
        :item-value="'value'"
        type="select"
        label-text="Type"
        @input="onThisFormInput($event, 'type')"
      />

      <v-checkbox v-model="dataFromFields.active" label="Active?" class="v-some-checkbox"></v-checkbox>

      <some-input
        v-if="hasGoodsField"
        :value="dataFromFields.goods"
        :items="optionItems1"
        :item-text="'text'"
        :item-value="'value'"
        type="multi-select"
        label-text="Goods"
        @input="onThisFormInput($event, 'goods')"
      />

      <some-input
        v-if="hasClientsField"
        :value="dataFromFields.clients"
        :items="optionItems1"
        :item-text="'text'"
        :item-value="'value'"
        type="multi-select"
        label-text="Clients"
        @input="onThisFormInput($event, 'clients')"
      />

      <some-input
        v-if="hasModificatorsField"
        :value="dataFromFields.modificators"
        :items="optionItems1"
        :item-text="'text'"
        :item-value="'value'"
        type="multi-select"
        label-text="Modificators"
        @input="onThisFormInput($event, 'modificators')"
      />

      <some-input
        v-if="dataFromFields.type"
        ref="someTextarea"
        :value="dataFromFields.temp"
        type="textarea"
        label-text="Temp"
        @input="onThisFormInput($event, 'temp')"
      />

      <some-input
        v-if="dataFromFields.type"
        ref="btns"
        :value="someMap"
        type="checkBtns"
        label-text="Labels"
        :selected-values="selectedValues"
        @input="onThisFormCheckInput"
      />

      <some-input :value="dataFromFields.pipe" label-text="Pipe" @input="onThisFormInput($event, 'pipe')" />
    </v-form>
    <div class="form-control-box">
      <v-btn :form="'home-view-form'" type="submit" class="v-form-btn">Submit</v-btn>
    </div>
  </div>
</template>
<script>
import SomeInput from '@/components/SomeInput/SomeInput.vue';

const INIT_FORM_ALL_VALUES = {
  name: '',
  description: '',
  type: '',
  active: false,
  goods: [],
  clients: [],
  modificators: [],
  temp: '',
  pipe: '',
};

const ACTIVE_CLIENTS_SELECTION_TYPES = ['Error Type', 'Ok Type', 'Remove Type'];
const CLIENTS_SELECTION_TYPES = [...ACTIVE_CLIENTS_SELECTION_TYPES, 'Clients Type'];
const ERRORS_TYPES = ['Dark Type', 'Error Type'];

export default {
  name: 'Home',
  components: {
    SomeInput,
  },
  props: {
    itemId: { type: [String, null], default: null },
    itemTypes: {
      type: Array,
      default: () => [
        { text: 'Clients Type', value: 'Clients Type' },
        { text: 'Dark Type', value: 'Dark Type' },
        { text: 'Remove Type', value: 'Remove Type' },
        { text: 'Ok Type', value: 'Ok Type' },
        { text: 'Error Type', value: 'Error Type' },
      ],
    },
  },
  data: () => ({
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
    dataFromFields: { ...INIT_FORM_ALL_VALUES },
  }),
  computed: {
    selectedValues() {
      return Object.values(this.someMap).filter(v => this.dataFromFields.temp.includes(v));
    },
    hasGoodsField() {
      return this.dataFromFields.type !== 'Clients Type';
    },
    hasClientsField() {
      return (
        (CLIENTS_SELECTION_TYPES.includes(this.dataFromFields.type) && this.dataFromFields.goods.length) ||
        this.dataFromFields.type === 'Clients Type'
      );
    },
    hasModificatorsField() {
      return ERRORS_TYPES.includes(this.dataFromFields.type) && this.dataFromFields.goods.length;
    },
  },
  mounted() {
    this.getTemplate();
    this.getSomeMap();
  },
  methods: {
    getTemplate() {
      this.dataFromFields.temp = 'line1: ${value_1}\nline2: ${value_2}\nline3:.';
    },
    getSomeMap() {
      this.someMap = { Key1: '${value_1}', Key2: '${value_2}', Key3: '${value_3}' };
    },
    onSubmit() {
      console.log('Submit');
      console.log('filtered: ', this.filterFields({ ...this.dataFromFields }));
    },
    onThisFormInput(event, field) {
      if (!field) return;
      this.dataFromFields[field] = event;
    },
    onThisFormCheckInput({ value, isChecked }) {
      const textareaEl = this.$refs.someTextarea?.$el.querySelector('textarea');
      if (!textareaEl) return;

      if (!isChecked) {
        this.dataFromFields.temp = this.template.replace(value, '');
        return;
      }

      const startCursorPosition = textareaEl.selectionStart;
      const endCursorPosition = textareaEl.selectionEnd;

      const beforeText = this.dataFromFields.temp.slice(0, startCursorPosition);
      const afterText = this.dataFromFields.temp.slice(endCursorPosition);
      const newTextareaValue = beforeText + value + afterText;

      this.dataFromFields.template = newTextareaValue;
    },
    filterFields(dataObj) {
      const { ...rest } = dataObj;

      const isValid = value => {
        if (Array.isArray(value)) {
          return value.length > 0;
        } else if (typeof value === 'boolean') {
          return true;
        }

        return Boolean(value);
      };

      return Object.fromEntries(Object.entries(rest).filter(([_, value]) => isValid(value)));
    },
  },
};
</script>
<style>
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
.v-some-checkbox {
  padding: 10px;
  border: 1px solid #fff;
  border-radius: 8px;
  background-color: rgb(212, 212, 212);
}
</style>
