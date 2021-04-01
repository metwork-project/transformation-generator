<template>
  <iframe src="ketcher/ketcher.html" width="100%" :height="height"></iframe>
</template>

<script>
export default {
  props: {
    height: {
      type: Number,
      default: 500,
      required: false
    }
  },
  data() {
    return { ketcher: null }
  },
  methods: {
    getKetcher: function() {
      if (!this.ketcher) {
        var thisId = this.$el.id
        var ketcherFrame = document.getElementById(thisId)
        if ('contentDocument' in ketcherFrame) {
          this.ketcher = ketcherFrame.contentWindow.ketcher
        }
        // IE7
        else this.ketcher = document.frames[thisId].window.ketcher
      }
    },
    getSmiles: function() {
      this.getKetcher()
      return this.ketcher.getSmiles()
    },
    setSmiles: function(smiles) {
      this.getKetcher()
      this.ketcher.setMolecule(smiles)
    }
  }
}
</script>
