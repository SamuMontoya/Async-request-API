<template>
  <b-container class="mt-5">
    <h1>Aplicación de divisas</h1>
    <div>
      <b-form @submit.prevent="handleSubmit">
        <label for="monto">Monto</label>
        <b-form-input
          type="number"
          min="1"
          id="monto"
          v-model="form.monto"
          :class="{ 'is-invalid': isSubmitted && $v.form.monto.$error }"
        ></b-form-input>
        <div v-if="isSubmitted && !$v.form.monto.required" class="invalid-feedback mb-2">Se requiere un monto</div>
        <div v-if="isSubmitted && !$v.form.monto.minValue" class="invalid-feedback mb-2">Se requiere un monto mayor a cero</div>

        <label for="input-list">Moneda de Origen</label>
        <b-form-select
          v-model="form.origen"
          list="input-list"
          :options="options"
          :class="{ 'is-invalid': isSubmitted && $v.form.origen.$error }"
        ></b-form-select>
        <div v-if="isSubmitted && !$v.form.origen.required" class="invalid-feedback mb-2">Se requiere un valor de origen</div>

        <label for="input-list-2">Moneda de Objetivo</label>
        <b-form-select
          v-model="form.objetivo"
          list="input-list-2"
          :options="options"
          :class="{ 'is-invalid': isSubmitted && $v.form.objetivo.$error }"
        ></b-form-select>
        <div v-if="isSubmitted && !$v.form.objetivo.required" class="invalid-feedback mb-2">Se requiere un valor de objetivo</div>

        <b-button class="mt-3" variant="primary" type="submit"
          >Submit</b-button
        >
      </b-form>
      <div v-if="isFull && $v.form.objetivo.required">
        <b-alert class="mt-5" show>
          {{ `${form.monto} ${form.origen} = ${valorFinal} ${form.objetivo} ` }}
        </b-alert>
      </div>

    </div>
  </b-container>
</template>

<script>
import {required, minValue} from "vuelidate/lib/validators"
import Empoints from "../components/Empoints.vue"

export default {
    name: "IndexPage",
    data() {
        return {
            form: {
                monto: null,
                origen: null,
                objetivo: null,
            },
            isSubmitted: false,
            isFull: false,
            options: [],
            siglas: [],
            nombres: [],
            valorFinal: null,
            siglaOrg: null,
            siglaObj: null,
            precios: []
        };
    },
    validations: {
        form: {
            monto: { required, minValue: minValue(1) },
            origen: { required },
            objetivo: { required }
        },
    },
    methods: {
        handleSubmit() {
            this.isSubmitted = true;
            this.$v.$touch();
            if (this.$v.$invalid) {
                return;
            }
            this.isFull = true

            this.valorFinal = this.form.monto / this.precios[this.form.origen] * this.precios[this.form.objetivo]

        },
        async getData() {
            const api = "https://openexchangerates.org/api/currencies.json?app_id=0f3e3db7c8bc483dbd8a79775ddc9de6";
            const response = await this.$axios.get(api);
            this.nombres = Object.values(response.data);
            this.siglas = Object.keys(response.data)

            for(let i = 1; i < this.nombres.length; i++){
              this.options.push({
                text:`${this.nombres[i]} (${this.siglas[i]})`, 
                value: this.siglas[i]
              })
            }

            const url = 'https://openexchangerates.org/api/latest.json?app_id=0f3e3db7c8bc483dbd8a79775ddc9de6'
            const res = await this.$axios.get(url)
            this.precios = res.data.rates
          }
    },
    async mounted() {
        await this.getData();
    },
    components: { Empoints }
}
</script>
