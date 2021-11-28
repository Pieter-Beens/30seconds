<template>
  <div class="kaartje container-fluid">
    
    <img class="w-100 img-fluid" src="./../assets/kaartje.jpg" />
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
  margin-left: 50%;
  font-size: 2.5vw;
  font: arial;
  font-weight: bold;
}
.kaartje {
  max-width: 750px;
  text-align: left;
  margin: auto;
}

img-fluid{
  max-width: 100%;
}

#textContainer {
  position: absolute;
  
  margin: auto;
}
</style>
