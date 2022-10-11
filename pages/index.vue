<template>
  <b-container class="mt-5">
    <h1>Aplicación de divisas</h1>
    <div>
      <b-form>
        <label for="monto">Monto</label>
        <b-form-input
          type="number"
          min="1"
          id="monto"
          v-model="monto"
        ></b-form-input>

        <label for="input-list">Moneda de Origen</label>
        <b-form-input
          v-model="origen"
          list="input-list"
          id="input-with-list"
        ></b-form-input>
        <b-form-datalist id="input-list" :options="options"></b-form-datalist>

        <label for="input-list-2">Moneda de Objetivo</label>
        <b-form-input
          v-model="objetivo"
          list="input-list-2"
          id="input-with-list-2"
        ></b-form-input>
        <b-form-datalist id="input-list-2" :options="options"></b-form-datalist>

        <b-button class="mt-3" variant="primary" @click="submitForm"
          >Submit</b-button
        >
      </b-form>
    </div>
  </b-container>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      monto: null,
      origen: null,
      objetivo: null,
      options: ["1", "2", "3", "4", "5"],
    };
  },

  methods: {
    submitForm() {
      alert("form-submited");
    },
  },
};
</script>
