<template>
  <div class="kaartje">
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
  width: 600px;
  height: 392px;
  background-image: url('./../assets/kaartje.jpg');
  text-align: left;
  margin: auto;
}
#textContainer {
  margin: 0;
  position: relative;
  top: 50%;
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);
}
</style>
