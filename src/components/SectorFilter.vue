<template>
  <v-card flat color="transparent">
    <v-subheader>Filter on sector</v-subheader>
    <v-combobox
      v-model="chips"
      :items="items"
      label="Selected sectors"
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
  props: ['sectors'],
  data() {
    return {
      chips: [
      ],
    };
  },
  computed: {
    items: function() {
      return this.sectors;
    }
  },
  watch: {
    chips: function(newChips){
      this.$emit("changedSectorFilter", {sectors: [...newChips]});
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