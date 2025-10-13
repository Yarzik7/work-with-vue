<template>
  <div class="input-wrap">
    <label :for="forId" class="label">{{ labelText }}</label>
    <v-textarea v-if="type === 'textarea'" :value="value" :id="forId" class="v-input-wrap" @input="onInput" />
    <v-text-field v-if="type === 'text'" :value="value" :id="forId" class="v-input-wrap" @input="onInput" />
    <v-btn-toggle v-if="type === 'checkBtns'" dense background-color="primary" dark multiple>
      <v-btn v-for="(item, index) in checkBtnEntries" :key="index" @input="onCheckInput($event, item)">
        {{ item[0] }}
      </v-btn>
    </v-btn-toggle>
    <v-select
      v-if="type === 'select'"
      :value="value"
      :id="forId"
      :items="items"
      :item-text="itemText"
      :item-value="itemValue"
      class="v-input-wrap"
      filled
      outlined
      @input="onInput"
    ></v-select>
    <v-autocomplete
      v-if="type === 'multi-select'"
      :value="value"
      :id="forId"
      :items="items"
      :item-text="itemText"
      :item-value="itemValue"
      class="v-input-wrap"
      @input="onInput"
      multiple
    ></v-autocomplete>
  </div>
</template>
<script>
export default {
  name: 'SomeInput',
  props: {
    type: {
      type: String,
      default: 'text',
    },
    labelText: {
      type: String,
      default: 'Some input',
    },
    value: {
      type: [String, Number, Array, Object],
      default: [],
    },
    items: {
      type: Array,
      default: () => [],
    },
    itemText: {
      type: String,
      default: 'text',
    },
    itemValue: {
      type: String,
      default: 'value',
    },
  },
  data() {
    return {
      forId: crypto.randomUUID(),
    };
  },
  computed: {
    checkBtnEntries() {
      return Object.entries(this.value);
    },
  },
  methods: {
    onInput(event) {
      this.$emit('input', event);
    },
    onCheckInput(event, item) {
      this.$emit('input', { value: item[1], isChecked: event });
    },
  },
};
</script>
<style scoped>
.label {
  display: block;
  margin-bottom: 10px;
}
.v-input-wrap {
  border: 1px solid #fff;
  border-radius: 8px;
  background-color: rgb(212, 212, 212);
}
</style>
