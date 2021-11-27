<template>
  <div class="hello">
    <h1>{{ concepts }}</h1>
    <p>
      Nu nog mooi maken.
    </p>
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  background-color: brown;
  margin: 40px 0 0;
}
</style>
