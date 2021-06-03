<template>
  <section class="section">
    <div class="columns is-desktop">
      <aside class="column is-3 border-grey">
        <p class="menu-label is-hidden-touch">
          Parameters
        </p>
        <SimplifierParams ref="params" class="menu-list">
          <b-field>
            <b-button type="is-primary" @click="update"
              >Generate Transformations</b-button
            >
          </b-field>
          <b-field>
            <a href="#" @click="loadExample"><i>Load example</i></a>
          </b-field>
        </SimplifierParams>
      </aside>
      <div class="column is-9">
        <Ketcher id="ifKetcherInput" ref="ketcher" />
      </div>
    </div>
    <div class="columns is-multiline">
      <TransformationView
        v-for="reaction in reactions"
        :key="reaction.id"
        :reaction-id="reaction.id"
        :smarts="reaction.smarts"
        :chem-doodle-json="reaction.chemDoodleJson"
        class="column is-full-tablet is-full-desktop is-half-widescreen"
        @load-smiles="loadSmiles"
      />
    </div>
  </section>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HomePage',
  data() {
    return { smiles: 'CCO', reactions: null }
  },
  methods: {
    update: function() {
      this.reactions = null
      let smiles = this.$refs.ketcher.getSmiles()
      let url =
        process.env.NUXT_ENV_CHEM_KIT_API_URL + '/transformations_from_smiles/'
      let smilesInput = smiles.split('.')
      axios
        .post(url, { smiles: smilesInput, params: this.$refs.params.params })
        .then((response) => {
          this.reactions = response.data
        })
    },
    loadExample: function() {
      let smiles =
        'Oc1ccc(cc1)-c1cc(=O)c2c(O)cc(O)c(O)c2o1.COc1cc(O)c2c(oc(cc2=O)-c2ccc(O)c(O)c2)c1O'
      this.loadSmiles(smiles)
    },
    loadSmiles: function(smiles) {
      this.$refs.ketcher.setSmiles(smiles)
      this.reactions = null
    }
  }
}
</script>
