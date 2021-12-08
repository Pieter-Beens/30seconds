<template>
  <div class="kaartje" @click="reveal()">
    <img
      class="img-fluid"
      v-if="!this.revealed"
      src="./../assets/kaartje_back.jpg"
    />
    <img
      class="img-fluid"
      v-if="this.revealed"
      src="./../assets/kaartje_front.jpg"
    />
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
    concepts: [],
  },
  data: function () {
    return {
      revealed: false,
    };
  },
  methods: {
    reveal() {
      this.revealed = true;
      this.$emit("revealed");
    },
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
  -webkit-box-shadow: 5px 5px 3px -1px rgba(0, 0, 0, 0.5);
  box-shadow: 5px 5px 3px -1px rgba(0, 0, 0, 0.5);
}

img-fluid {
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
