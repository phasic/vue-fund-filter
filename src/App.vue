<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Fund filter</span>
        <span class="font-weight-light">Let's make some nice filters</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>

    <v-content>
      <div class="padding-top">
        <v-container grid-list-md text-xs-center>
          <v-layout row wrap>
            <v-flex xs6>
              <SectorFilter
                v-on:changedSectorFilter="sectorFilterChanged"
                v-bind:sectors="sectors"
              />
            </v-flex>
            <v-flex xs6>
              <CurrencyFilter
                v-on:changedCurrencyFilter="currencyFilterChanged"
                v-bind:currencies="currencies"
              />
            </v-flex>
          </v-layout>
        </v-container>
        <ValuationFilter
          v-on:changedValuationFilter="valuationFilterChanged"
          v-bind:min="min"
          v-bind:max="max"
          v-bind:selectedMin="selectedMin"
        />
        <DataTable v-bind:instruments="filteredData"/>
      </div>
    </v-content>
  </v-app>
</template>

<script>
import ValuationFilter from "./components/ValuationFilter";
import SectorFilter from "./components/SectorFilter";
import CurrencyFilter from "./components/CurrencyFilter";
import DataTable from "./components/DataTable";
import MOCKDATA from "./assets/mock-data.js";

export default {
  name: "App",
  components: {
    ValuationFilter,
    SectorFilter,
    CurrencyFilter,
    DataTable
  },
  data() {
    return {
      data: MOCKDATA,
      min: null,
      max: null,
      selectedMin: null,
      selectedMax: null,
      sectors: null,
      selectedSectors: null,
      currencies: null,
      selectedCurrencies: null
    };
  },
  created() {
    ({
      min: this.min,
      min: this.selectedMin,
      max: this.max,
      max: this.selectedMax,
      sectors: this.sectors,
      sectors: this.selectedSectors,
      currencies: this.currencies,
      currencies: this.selectedCurrencies
    } = this._getDefaultValues());
  },
  computed: {
    filteredData: function() {
      return this._filterData(
        this.selectedMin,
        this.selectedMax,
        this.selectedSectors,
        this.selectedCurrencies
      );
    }
  },
  methods: {
    _getDefaultValues() {
      let sectors, currencies;
      let {
        0: { valuation: min },
        [MOCKDATA.length - 1]: { valuation: max }
      } = MOCKDATA.sort((x, y) => x.valuation - y.valuation);

      sectors = [...new Set(MOCKDATA.map(el => el.sector))];
      currencies = [...new Set(MOCKDATA.map(el => el.currency))];
      return {
        min: Math.floor(min),
        max: Math.ceil(max),
        sectors,
        currencies
      };
    },
    valuationFilterChanged(payload) {
      [this.selectedMin, this.selectedMax] = payload.range;
    },
    sectorFilterChanged(payload) {
      this.selectedSectors =
        payload.sectors.length < 1 ? this.sectors : [...payload.sectors];
    },
    currencyFilterChanged(payload) {
      this.selectedCurrencies =
        payload.currencies.length < 1
          ? this.currencies
          : [...payload.currencies];
    },
    _filterData(min, max, sectors, currencies) {
      let filteredData = MOCKDATA;
      const minMaxFilter = ({ key, min, max }, el) =>
        el[key] >= min && el[key] <= max;

      const arrayFilter = ({ key, array }, el) =>
        array && array.indexOf(el[key]) !== -1;

      filteredData = filteredData
        .filter(minMaxFilter.bind(this, { key: "valuation", min, max }))
        .filter(arrayFilter.bind(this, { key: "currency", array: currencies }))
        .filter(arrayFilter.bind(this, { key: "sector", array: sectors }));
      return filteredData;
    }
  }
};
</script>

<style scoped>
.padding-top {
  padding-top: 50px;
}
</style>

