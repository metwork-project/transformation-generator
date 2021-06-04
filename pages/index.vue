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
        .post(url, {
          smiles: smilesInput,
          reverse: true,
          params: this.$refs.params.params
        })
        .then((response) => {
          this.reactions = response.data
        })
    },
    loadExample: function() {
      let smiles =
        'OC1=CC=C(C=C1)C1=CC(=O)C2=C(O)C=C(O)C(O)=C2O1.COC1=CC(O)=C2C(=O)C=C(OC2=C1O)C1=CC=C(O)C(O)=C1'
      this.loadSmiles(smiles)
    },
    loadSmiles: function(smiles) {
      this.$refs.ketcher.setSmiles(smiles)
      this.reactions = null
    }
  }
}
</script>
