<template>
  <div class="kaartje">
    <img class="img-fluid" src="./../assets/kaartje.jpg"/>
    <div id="textContainer" >
      <p v-for="concept in concepts" v-bind:key="concept">{{ concept.toUpperCase() }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Kaartje',
  props: {
    concepts: []
  }
}
</script>

<style scoped>
p {
  margin-left: 120px;
  font-size: 28px;
  font: arial;
  font-weight: bold;
}
.kaartje {
  width: 800px;
  text-align: left;
  margin: auto;
}

img{
  display: block;
  width: 100%;
  height: 100%;
}
#textContainer {
  position: absolute;
        z-index: 999;
        margin: 0 auto;
        left: 0;
        right: 0;
        top: 25%;
        width: 60%;
}
</style>
