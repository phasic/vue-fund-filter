<template>
  <div>
    <v-data-table :headers="headers" :items="instruments" class="elevation-1">
      <template v-slot:items="props">
        <td>{{ props.item.isin }}</td>
        <td class="text-xs-left">{{ props.item.currency }}</td>
        <td class="text-xs-left">{{ props.item.valuation }}</td>
        <td class="text-xs-left">{{ props.item.sector }}</td>
      </template>
    </v-data-table>
    <pre></pre>
  </div>
</template>
<script>
export default {
  props: ["instruments"],
  data() {
    return {
      filteredInstruments: [],
      headers: [
        {
          text: "ISIN",
          align: "left",
          sortable: false,
          value: "isin"
        },
        { text: "Currency", align: "left", value: "currency" },
        { text: "Valuation", align: "left", value: "valuation" },
        { text: "Sector", align: "left", value: "sector" }
      ]
    };
  }
};
</script>