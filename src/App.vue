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
         <RangeSlider
          v-on:changedSelection="rangeChanged"
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
import RangeSlider from "./components/RangeSlider";
import SectorFilter from "./components/SectorFilter";
import CurrencyFilter from "./components/CurrencyFilter";
import DataTable from "./components/DataTable";
import _ from "lodash";
import MOCKDATA from "./assets/mock-data.js";

export default {
  name: "App",
  components: {
    RangeSlider,
    SectorFilter,
    CurrencyFilter,
    DataTable
  },
  data() {
    return {
      min: this._getDefaultMinimum(),
      max: this._getDefaultMaximum(),
      selectedMin: null,
      selectedMax: null,
      sectors: this._getDefaultSectors(),
      selectedSectors: this._getDefaultSectors(),
      currencies: this._getDefaultCurrencies(),
      selectedCurrencies: this._getDefaultCurrencies()
    };
  },
  computed: {
    data: function() {
      return this._getMockData();
    },
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
    _getMockData() {
      return MOCKDATA;
    },
    _getDefaultMinimum() {
      return Math.min.apply(
        Math,
        MOCKDATA.map(function(o) {
          return Math.floor(o.valuation);
        })
      );
    },
    _getDefaultMaximum() {
      return Math.max.apply(
        Math,
        MOCKDATA.map(function(o) {
          return Math.ceil(o.valuation);
        })
      );
    },
    _getDefaultSectors() {
      return [...new Set(MOCKDATA.map(el => el.sector))];
    },
    _getDefaultCurrencies() {
      return [...new Set(MOCKDATA.map(el => el.currency))];
    },
    rangeChanged(payload) {
      [this.selectedMin, this.selectedMax] = payload.range;
    },
    sectorFilterChanged(payload) {
      this.selectedSectors = [...payload.sectors];
    },
    currencyFilterChanged(payload) {
      console.log("in currencyfilterchanged");
      this.selectedCurrencies = [...payload.currencies];
    },
    _filterData(min, max, sectors, currencies) {
      const valuationFiltered = this.data.filter(
        el => el.valuation >= min && el.valuation <= max
      );
      const sectorFiltered =
        sectors.length === 0
          ? valuationFiltered
          : valuationFiltered.filter(
              el => sectors && sectors.indexOf(el.sector) !== -1
            );
      const currencyFiltered =
        currencies.length === 0
          ? sectorFiltered
          : sectorFiltered.filter(
              el => currencies && currencies.indexOf(el.currency) !== -1
            );
      return currencyFiltered;
    }
  }
};
</script>

<style scoped>
.padding-top {
  padding-top: 50px;
}
</style>

