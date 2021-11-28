<template>
  <div class="kaartje">
    
    <img class=" img-fluid" src="./../assets/kaartje.jpg" />
    <div id="textContainer">
      <p v-for="concept in concepts" v-bind:key="concept">
        {{ concept.toUpperCase() }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Kaartje",
  props: {
    concepts: [],
  },
};
</script>

<style scoped>
p {
  font-size: 3vw;
  font: arial;
  font-weight: bold;
}
.kaartje {
  position: relative;
  max-width: 750px;
  text-align: left;
  margin: auto;
}

img-fluid{
  max-width: 70%;
}

#textContainer {
  position: absolute;
  z-index: 999;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
