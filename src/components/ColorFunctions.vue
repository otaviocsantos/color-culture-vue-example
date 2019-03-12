// ColorFunctions.vue

<template>
  <div>
    <div class="breather">
      <h2>Color Manipulation</h2>
      <p>Every instance of the Color class contains commonly used functions for color manipulation.</p>
      <pre> 
      let color = new {{stringed}};
      console.log(color.rgba);
      console.log(color.lighten().rgba);
    </pre>
      <div class="group">
        <Sample :color="currentColor"/>
        <Sample :color="currentColor.lighten()"/>
      </div>
      <p>Modifications can also be chained.</p>
      <pre> 
      let color = new {{stringed}};
      console.log(color.rgba);
      console.log(color.darken().desaturate());
    </pre>
      <div class="group">
        <Sample :color="currentColor"/>
        <Sample :color="currentColor.darken().desaturate()"/>
      </div>
      <p>With the exception of negate a modification can be changed an amount.</p>
      <pre> 
      let color = new {{stringed}};
      console.log(color.rgba); 
      console.log(color.rotate(120).saturate(55));
    </pre>
      <div class="group">
        <Sample :color="currentColor"/>
        <Sample :color="currentColor.rotate(60).saturate(55)"/>
      </div>
      <p>
        A modification
        <strong>DO NOT</strong> change the original color, if you do wish to change it reassign the result of the modification like so:
      </p>
      <pre> 
      let color = new {{stringed}};
      color.negate(); // this will NOT  change the value of the variable color
      color = color.negate(); // this will update the value of the variable color
    </pre>
    </div>
    <h3>Color manipulation playground</h3>

    <div class="group">
      <Sample :color="currentColor"/>
      <CMYKPicker :color="currentColor" v-on:changed="updateColor"/>
    </div>
    <div class="playground breather">
      <div class="desc">
        <h5>rotate 25&deg;</h5>
        <Sample :color="currentColor.rotate(25)"/>
      </div>
      <div class="desc">
        <h5>darken</h5>
        <Sample :color="currentColor.darken()"/>
      </div>
      <div class="desc">
        <h5>lighten</h5>
        <Sample :color="currentColor.lighten()"/>
      </div>
      <div class="desc">
        <h5>whiten</h5>
        <Sample :color="currentColor.whiten()"/>
      </div>
      <div class="desc">
        <h5>grayscale</h5>
        <Sample :color="currentColor.grayscale()"/>
      </div>
      <div class="desc">
        <h5>blacken</h5>
        <Sample :color="currentColor.blacken()"/>
      </div>
      <div class="desc">
        <h5>saturate</h5>
        <Sample :color="currentColor.saturate()"/>
      </div>
      <div class="desc">
        <h5>desaturate</h5>
        <Sample :color="currentColor.desaturate()"/>
      </div>
      <div class="desc">
        <h5>negate</h5>
        <Sample :color="currentColor.negate()"/>
      </div>
    </div>
  </div>
</template>
<style scoped>
.playground {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.desc {
  border: 1px solid #cacaca;
  width: 84px;
  padding: 0 0 5px 5px;
  margin: 0 15px 15px 0;
}

.group {
  padding: 3em;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  align-content: space-around;
  align-items: flex-start;
}
Sample {
  order: 0;
  flex: 0 1 auto;
  align-self: auto;
}
</style>

<script>
import { Color } from "color-culture";
import CMYKPicker from "./MiniPicker/CMYKPicker.vue";
import Sample from "./Sample.vue";

export default {
  components: {
    CMYKPicker,
    Sample
  },
  props: {},
  data: function() {
    return {
      label: "",
      stringed: "Color('Turquoise')",
      currentColor: Color
    };
  },
  computed: {
    value: {
      get: () => {
        return this.currentColor;
      },
      set(val) {
        this.currentColor = val;
      }
    }
  },
  methods: {
    updateColor: function(value) {
      this.currentColor.channels = value.channels;
      this.stringed = `RGB('${this.currentColor.rgb().rgba}')`;
    }
  },
  beforeMount: function() {
    this.currentColor = new Color("Turquoise").cmyk();
  },
  beforeCreate: function() {}
};
</script>