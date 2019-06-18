<style>
.slider {
  display: flex;
  margin: 0 auto;
}

label {
  width: 80px;
  display: block;
}

input {
  margin-left: 8px;
  
}

li {
  flex: 0 1 25%;
  list-style: none;
  position: relative;
}

li:not(:last-child):after {
  content: "";
  position: absolute;
  height: 2px;
  width: 100%;
  background: rgb(55,118,202);
  top: 32px;
}
</style>

<template>
  <v-card flat color="transparent">
    <v-subheader>Filter on currency</v-subheader>

    <ul class="slider">
      <li class="slider-item">
        <label for="f-option">All</label>
        <input type="radio" id="f-option" name="rb">
      </li>
      <li class="slider-item">
        <label for="g-option">EUR</label>
        <input type="radio" id="g-option" name="rb">
      </li>
      <li class="slider-item">
        <label for="k-option">JPY</label>
        <input type="radio" id="k-option" name="rb">
      </li>
      <li class="slider-item">
        <label for="k-option">USD</label>
        <input type="radio" id="k-option" name="rb">
      </li>
    </ul>
  </v-card>
</template>
<script>
export default {};
</script>