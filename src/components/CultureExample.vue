// CultureExample.vue

<template>
  <div>
    <h2>How Color Culture works</h2>
    <p>A culture is like a color palette, it holds colors together:</p>
    <CultureComp :value="simple"/>
    <div class="breather">
      <p>The main difference is that in a culture you may have dynamic collors.</p>
      <p>This is a culture:</p>
    </div>

    <div class="relation">
      <div class="source">
        <Sample ref="cmykSample" :color="cmykSource"/>
      </div>
      <div class="plus">
        <h1>+</h1>
      </div>
      <div class="modifier">
        <CMYKPicker ref="cmykPicker" :color="cmykPickerInit" v-on:changed="updateCMYK"/>
      </div>
      <div class="equal">
        <h1>=</h1>
      </div>
      <div class="result">
        <Sample ref="cmykSample" :color="cmykSource"/>
      </div>
      <div class="warn">
        <h6>drag the color bars to change how the color is affected</h6>
      </div>
    </div>

    <div class="breather" >
      <p>It has a color (yellow) and a modifier: {{cmykSource.cmyk().toString()}}, Color Culture then takes the first color, adds the modifier to it and generates the second color.</p>
      <p>That means that if you alter the first color or the modfier, the second color will be automatically updated.</p>
      <p>You are not limited to CMYK, you may use RGB, HSL, LAB, XYZ or create your own color model.</p>

      <p>In this example we have an LAB color, modified by an RGB color which is modified by an HSL color:</p>
    </div>
    <CultureComp class="breather" :value="chained"/>

    <div class="relation">
      <div class="line">
        <h3>RGB</h3>
      </div>
      <div class="source">
        <Sample ref="cmykSample" :color="chained.items[0].result"/>
      </div>
      <div class="plus">
        <h1>+</h1>
      </div>
      <div class="modifier">
        <RGBPicker :color="rgbPickerInit" v-on:changed="updateRGB"/>
      </div>
      <div class="equal">
        <h1>=</h1>
      </div>
      <div class="result">
        <Sample ref="cmykSample" :color="chained.items[1].result"/>
      </div>
      <div class="warn">
        <h6>drag the color bars to change how the color is affected</h6>
      </div>
    </div>

    <div class="relation breather">
      <div class="line">
        <h3>HSL</h3>
      </div>
      <div class="source">
        <Sample ref="cmykSample" :color="chained.items[1].result"/>
      </div>
      <div class="plus">
        <h1>+</h1>
      </div>
      <div class="modifier">
        <HSLPicker :color="hslPickerInit" v-on:changed="updateHSL"/>
      </div>
      <div class="equal">
        <h1>=</h1>
      </div>
      <div class="result">
        <Sample ref="cmykSample" :color="chained.items[2].result"/>
      </div>
      <div class="warn">
        <h6>drag the color bars to change how the color is affected</h6>
      </div>
    </div>
  </div>
</template>
<style scoped>
@import "./CultureExample.css";
</style>

<script>
import { CMYK, Color, Culture, HSL, RGB, LAB } from "color-culture";
import CultureComp from "./CultureComp.vue";
import CMYKPicker from "./MiniPicker/CMYKPicker.vue";
import HSLPicker from "./MiniPicker/HSLPicker.vue";
import RGBPicker from "./MiniPicker/RGBPicker.vue";
import Sample from "./Sample.vue";

export default {
  components: {
    CultureComp,
    CMYKPicker,
    HSLPicker,
    RGBPicker,
    Sample
  },
  data: function() {
    return {
      simple: Culture,
      cmykSource: Color,
      labSource: LAB,
      rgbPickerInit: RGB,
      hslPickerInit: HSL,

      chained: Culture
    };
  },
  methods: {
    updateCMYK(value) {
      this.cmykSource = new Color(value.rgb().toString());
    },
    updateRGB(value) {
      this.chained.items[1].modifier.channels = value.channels;
    },
    updateHSL(value) {
      this.chained.items[2].modifier.channels = value.channels;
    }
  },

  beforeCreate() {},

  beforeMount() {
    this.parentObj = { title: "beforeMount" };

    this.simple = new Culture();
    this.cmykSource = new Color("Gold").to("cmyk");
    this.cmykPickerInit = new CMYK([20, 80, 10, 16.25, 1]);

    const simpleRelation0 = this.simple.addColor(this.cmykSource);
    this.simple.addRelation(this.cmykPickerInit, simpleRelation0);

    this.labSource = new LAB([15, 33, 11, 1]);

    this.chained = new Culture();
    this.rgbPickerInit = new RGB([20, 25, 18, 0]);
    this.hslPickerInit = new HSL([130, 80, 30, 0]);

    const origin = this.chained.addColor(new LAB([15, 33, 11, 1]));
    const rgbRelation = this.chained.addRelation(
      new RGB([20, 25, 18, 0]),
      origin
    );
    this.chained.addRelation(new HSL([30, 0, 30, 0]), rgbRelation);
  }
};
</script>