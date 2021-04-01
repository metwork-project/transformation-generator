<template>
  <ul>
    <b-field v-for="(param, name) in paramsSpec" :key="param.title">
      <b-switch v-model="params[name]">{{ param.title }}</b-switch>
      <b-tooltip :label="param.description" type="is-light" position="is-right">
        <b-icon icon="information"> </b-icon>
      </b-tooltip>
    </b-field>
    <slot></slot>
  </ul>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return { params: {}, paramsSpec: null }
  },
  mounted() {
    let urlspec = process.env.NUXT_ENV_CHEM_KIT_API_URL + '/openapi.json'
    axios.get(urlspec).then((response) => {
      this.paramsSpec =
        response.data.components.schemas.SimplifierParams.properties
      Object.keys(this.paramsSpec).map((key) => {
        let spec = this.paramsSpec[key]
        this.params[key] = spec.default
      })
    })
  }
}
</script>
