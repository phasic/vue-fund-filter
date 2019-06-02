<template>
  <v-card flat color="transparent">
    <v-subheader>Filter on valuation</v-subheader>

    <v-card-text>
      <v-layout row>
        <v-flex shrink style="width: 60px">
          <v-text-field v-model="range[0]" class="mt-0" hide-details single-line type="number"></v-text-field>
        </v-flex>
        <v-flex class="px-3">
          <v-range-slider v-model="range" :max="max" :min="min"></v-range-slider>
        </v-flex>
        <v-flex shrink style="width: 60px">
          <v-text-field v-model="range[1]" class="mt-0" hide-details single-line type="number"></v-text-field>
        </v-flex>
      </v-layout>
    </v-card-text>
  </v-card>
</template>
<script>
export default {
  props: ["min", "max"],
  data() {
    return {
      range: [null, null]
    };
  },
  watch: {
    range: function(newRange) {
      this.$emit("changedValuationFilter", {range: newRange});
    }
  },
  created() {
    this.range = [this.min, this.max];
  }
};
</script>