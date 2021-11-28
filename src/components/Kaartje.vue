<template>
  <div class="kaartje container-fluid" @click="reveal()">
    <img class="w-100 img-fluid" v-if="!this.revealed" src="./../assets/kaartje_back.jpg" />
    <img class="w-100 img-fluid" v-if="this.revealed" src="./../assets/kaartje_front.jpg" />
    <div id="textContainer" v-if="this.revealed">
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
    concepts: []
  },
  data: function() {
    return {
      revealed: false
    }
  },
  methods: {
    reveal() {
      this.revealed = true;
      this.$emit('revealed');
    }
  }
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
