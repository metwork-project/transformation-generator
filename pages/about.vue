<template>
  <section class="section content">
    <p class="title is-2">MetWork Transformation Generator</p>
    <p class="title is-3">Context</p>
    <p>
      This web app aimed to help
      <ExtLink url="https://metwork.science">MetWork web app</ExtLink> users to
      easily generate valid and usefull transformations (also called
      "Reactions").
    </p>
    <p class="title is-4">Why not directly integrated to Metwork app ?</p>
    <p>
      MetWork team is preparing a major new version in the months to come. This
      new version will imply breaking changes in favor of users feedback and new
      scientific and technological improvement. The web technological stack will
      radically change and it has been found more efficient to temporarly
      propose this interface based on this new stack rather than change the
      current app that will be soone obsolete.
    </p>
    <p class="title is-4">Reaction or Transformation ?</p>
    <p>
      Reaction has a definition in chemical context that not fit to metwork
      transformation for those two main reasons:
    </p>
    <ul>
      <li>A transformation is not necessarily chemically balanced.</li>
      <li>
        A transformation can consist of the succession of more than one chemical
        reactions.
      </li>
    </ul>
    <p>
      For MetWork v2 the wording will change and transformation will be used
      instead of reaction.
    </p>
    <p class="title is-4">What are the improvements ?</p>
    <ul>
      <li>
        <b>No mapping need</b> : just draw a couple of smiles and an algorithm
        will automatically map atoms
      </li>
      <li>
        <b>Simplify to "minimal" transformation</b> : you can input complete
        molecule structure and the algorithm will find "simplified"
        transformation(s) that explain the difference between the two strucutre
        (use "load example" in the interface)
      </li>
    </ul>
    <p class="title is-4">How to use in MetWork ?</p>
    <ol>
      <li>
        Check the proposed transformations to keep only those consistent.
      </li>
      <li>
        Click on "Copy SMARTS" button of the transformation.
      </li>
      <li>
        On MetWork reactions interface, create a new reaction and use the "Load
        SMARTS" button to past the SMARTS you copied.
      </li>
    </ol>
    <p class="title is-3">References</p>
    <p>Here's the technical background for thoses insterested in ;-)</p>
    <p>
      This web app is based on Python including homemade chemical and web
      packages.
    </p>
    <p class="title is-4">ChemKit Python package</p>
    <p>
      A chemical toolbox based on RDKit. It is under development and will be an
      important tool for MetWork v2.
    </p>
    <ul>
      <li>
        <ExtLink url="http://chem-kit.metwork.science/">Documentation</ExtLink>
      </li>
      <li>
        <ExtLink url="https://github.com/YannBeauxis/chem-kit"
          >Source code</ExtLink
        >
      </li>
    </ul>
    <p class="title is-4">ChemKit API</p>
    <p>
      Expose the ChemKit package on a web REST API based on FastAPI. This API is
      deployed as a micro-service using Docker and Kubernetes.
    </p>
    <ul>
      <li>
        <ExtLink :url="apiDocUrl">Documentation</ExtLink>
      </li>
      <li>
        <ExtLink url="https://github.com/YannBeauxis/chem-kit-api"
          >Source code</ExtLink
        >
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  computed: {
    apiDocUrl: function() {
      return process.env.NUXT_ENV_CHEM_KIT_API_URL + '/docs'
    }
  }
}
</script>
