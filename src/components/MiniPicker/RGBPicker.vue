// RGBPicker.vue

<template>
  <div>
    <ul v-if="color" id="example">
      <li class='track' v-for="(item, index) in fill" :key="index">
        <Track
          :index="index"
          :trackWidth="width"
          :trackHeight="height/fill.length"
          :range="[0,255]"
          :fill="item"
          :value="color.channels[index]"
          v-on:changed="setChannel"
        />
      </li>
    </ul>
  </div>
</template>
<style scoped>
@import "./picker.css";
</style>

<script>
import Track from "./Track.vue";

export default {
  components: {
    Track
  },
  props: ["color"],
  data() {
    return {
      width: 80,
      height: 80,
      mousedown: false,
      fill: [
        [{ i: 0, c: "red" }, { i: 1, c: "red" }],
        [{ i: 0, c: "green" }, { i: 1, c: "green" }],
        [{ i: 0, c: "blue" }, { i: 1, c: "blue" }]
      ]
    };
  },
  computed: {},
  methods: {
    setChannel(value = 0, index = 0) {
      value = value * 1;
      if (isNaN(value)) {
        throw new Error("Channel set value invalid");
      }
      index = index * 1;
      if (isNaN(index) || index < 0 || index > this.color.channels.length) {
        throw new Error("Index set value invalid");
      }
      this.color.channels[index] = value;
      this.refresh();
    },
    refresh() {
      this.$emit("changed", this.color);
    }
  },

  beforeMount() {},

  mounted() {}
};
</script>