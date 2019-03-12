// Track.vue

<template>
  <div>
    <canvas
      ref="bar"
      :width="trackWidth"
      :height="trackHeight"
      @mousedown="onMouseDown"
      @mousemove="onMouseMove($event)"
      @mouseup="onMouseUp"
    ></canvas>
  </div>
</template>
<style scoped>
</style>

<script>
export default {
  props: {
    index: Number,
    fill: Array,
    trackWidth: Number,
    trackHeight: Number,
    range: Array,
    endPad: Number,
    value: Number
  },
  data: function() {
    return {
      mousedown: false,
      innerValue:0,
    };
  },
  methods: {
    onMouseDown(event) {
      this.mousedown = true;
      this.setChannel((this.range[1] * event.offsetX) / this.trackWidth);
    },
    onMouseMove(event) {
      if (this.mousedown) {
        this.setChannel((this.range[1] * event.offsetX) / this.trackWidth);
      }
    },
    onMouseUp() {
      this.mousedown = false;
    },
    setChannel(val = 0) {
      this.innerValue = val * 1;
      if (isNaN(this.innerValue)) {
        throw new Error("Channel set value invalid");
      }
      this.innerValue =
        this.innerValue < this.range[0]
          ? this.range[0]
          : this.innerValue > this.range[1]
          ? this.range[1]
          : this.innerValue;
      this.draw(this.innerValue, this.fill);
      this.$emit("changed", this.innerValue, this.index);
    },

    draw(val, fill) {
      const ctx = this.$refs.bar.getContext("2d");
      if (val != undefined && fill && ctx) {
        if (ctx) {
          ctx.clearRect(0, 0, this.trackWidth, this.trackHeight);
          const gradient = ctx.createLinearGradient(0, 0, this.trackWidth, 0);
          fill.map(o => {
            gradient.addColorStop(o.i, o.c);
          });

          if (val) {
            ctx.beginPath();
            ctx.fillStyle = gradient;
            ctx.rect(
              0,
              0,
              (val / this.range[1]) * this.trackWidth,
              this.trackHeight
            );
            ctx.fill();
            ctx.closePath();
          }
        }
      }
    }
  },

  mounted() {
    window.addEventListener("mouseup", this.onMouseUp);
    this.draw(this.value, this.fill);
  }
};
</script>