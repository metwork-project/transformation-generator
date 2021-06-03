<template>
  <div class="is-relative">
    <canvas
      :id="canvasId"
      class="ChemDoodleWebComponent"
      :width="width"
      :height="height"
      alt="ChemDoodle Web Component"
      style="background-color: rgb(255, 255, 255); border: 1px solid #ccc"
      >This browser does not support HTML5/Canvas.</canvas
    >
    <div
      class="is-flex is-flex-direction-row"
      style="position: absolute; left: 20px; bottom: 25px"
    >
      <b-button
        type="is-primary"
        size="is-small"
        class="mx-1"
        @click="loadSmarts"
      >
        Edit
      </b-button>
      <b-button
        v-clipboard:copy="smarts"
        v-clipboard:success="onCopy"
        type="is-primary"
        size="is-small"
        class="mx-1"
      >
        Copy SMARTS
      </b-button>
      <!-- <div class="notification ml-1 p-1 is-size-7 is-success is-light">
        SMARTS copied to clipboard
      </div> -->
      <b-notification
        v-model="smartsCopied"
        class="notification mx-1 py-1 px-2 is-size-7 is-success is-light"
        :closable="false"
        auto-close
        type="is-success"
        aria-close-label="Close notification"
      >
        SMARTS copied to clipboard
      </b-notification>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: {
    reactionId: {
      type: Number,
      required: true
    },
    chemDoodleJson: {
      type: Object,
      required: true
    },
    smarts: {
      type: String,
      required: true
    },
    width: {
      type: Number,
      default: 600
    },
    height: {
      type: Number,
      default: 300
    }
  },
  data() {
    return { smartsCopied: false }
  },
  computed: {
    canvasId: function() {
      return 'ChemDoodleCanvas' + this.reactionId
    }
  },
  mounted() {
    let ChemDoodleCanvas = new ChemDoodle.ViewerCanvas(
      this.canvasId,
      this.widht,
      this.height
    )
    ChemDoodleCanvas.styles.atoms_useJMOLColors = true
    let dataJSON = this.chemDoodleJson
    let jsi = new ChemDoodle.io.JSONInterpreter()
    let target = jsi.contentFrom(dataJSON)
    if (!this.noLabel) {
      var l = 0
      target.shapes.map(function(shape) {
        shape.label = l
        shape.error = true
        l += 1
      })
    }
    ChemDoodleCanvas.loadContent(target.molecules, target.shapes)
  },
  methods: {
    onCopy: function() {
      this.smartsCopied = true
    },
    loadSmarts: function() {
      let url = process.env.NUXT_ENV_CHEM_KIT_API_URL + '/smiles_from_smarts'
      axios.post(url, { smarts: this.smarts }).then((response) => {
        let smiles = response.data.join('.')
        this.$emit('load-smiles', smiles)
      })
    }
  }
}
</script>
