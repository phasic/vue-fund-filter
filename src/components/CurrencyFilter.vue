<template>
  <v-card flat color="transparent">
    <v-subheader>Filter on currency</v-subheader>
    <v-combobox
      v-model="chips"
      :items="currencies"
      label="Selected currencies"
      chips
      clearable
      solo
      multiple
    >
      <template v-slot:selection="data">
        <v-chip :selected="data.selected" close @input="remove(data.item)">
          <strong>{{ data.item }}</strong>
        </v-chip>
      </template>
    </v-combobox>
  </v-card>
</template>
<script>
export default {
  props: ["currencies"],
  data() {
    return {
      chips: []
    };
  },
  watch: {
    chips: function(currencies) {
      this.$emit("changedCurrencyFilter", { currencies });
    }
  },
  methods: {
    remove(item) {
      this.chips.splice(this.chips.indexOf(item), 1);
      this.chips = [...this.chips];
    }
  }
};
</script>