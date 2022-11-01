<!-- Please remove this file from your project -->
<template>
  <div>
    <button @click="divisaPrecio">Divisas con precio</button>
    <p>{{ precios }}</p>
    <br />
    <button @click="divisaNombres">Nombres Divisas</button>
    <p>{{ nombres }}</p>
  </div>
</template>

<script>
export default {
  name: 'NuxtTutorial',

  data(){
    return {
      precios:null,
      nombres:null,
    }
  },
  methods: {
    async divisaPrecio() {
      const api = 'https://openexchangerates.org/api/latest.json?app_id=0f3e3db7c8bc483dbd8a79775ddc9de6'
      const response = await this.$axios.get(api)
      this.precios = response
    },
    async divisaNombres() {
      const api = 'https://openexchangerates.org/api/currencies.json?app_id=0f3e3db7c8bc483dbd8a79775ddc9de6'
      const response = await this.$axios.get(api)
      this.nombres = response
    }
  }
}
</script>
