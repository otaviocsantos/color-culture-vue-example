// HSLPicker.vue

<template>
  <div>
    <ul v-if="color">
      <li class='track' v-for="(n, index) in 3" :key="index">
        <Track
          ref="track"
          :index="index"
          :trackWidth="width"
          :trackHeight="height/3"
          :range="color.ranges[index]"
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
import { HSL } from "color-culture";
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
      copy: HSL
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

      this.copy = this.color.to("hsl");
      this.copy.channels[1] = 100;
      this.copy.channels[2] = 50;
      this.copy.channels[3] = 100;

      this.$refs.track[0].draw(this.color.channels[0], this.hue());
      this.$refs.track[1].draw(this.color.channels[1], this.saturation());
      this.$refs.track[2].draw(this.color.channels[2], this.lightness());

      this.refresh();
    },
    refresh() {
      this.$emit("changed", this.color);
    },
    hue() {
      return [
        { i: 0, c: "rgba(255, 0, 0, 1)" },
        { i: 0.17, c: "rgba(255, 255, 0, 1)" },
        { i: 0.34, c: "rgba(0, 255, 0, 1)" },
        { i: 0.51, c: "rgba(0, 255, 255, 1)" },
        { i: 0.68, c: "rgba(0, 0, 255, 1)" },
        { i: 0.85, c: "rgba(255, 0, 255, 1)" },
        { i: 1, c: "rgba(255, 0, 0, 1)" }
      ];
    },
    saturation() {
      return [
        { i: 0, c: "rgba(128, 128, 128, 1)" },
        { i: 1, c: this.copy.rgba }
      ];
    },
    lightness() {
      return [
        { i: 0, c: "rgba(0, 0, 0, 1)" },
        { i: 0.5, c: this.copy.rgba },
        { i: 1, c: "rgba(255, 255, 255, 1)" }
      ];
    }
  },

  beforeMount() {},

  mounted() {
    this.setChannel(this.color.channels[0], 0);
  }
};
</script>