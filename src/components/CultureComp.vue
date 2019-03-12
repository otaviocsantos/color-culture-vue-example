// CultureComp.vue

<template>
  <div v-if="value">
    <ul class="group" v-if="value.items">
      <li class="line" v-for="(item, index) in value.items" :key="index">
        <div class="sample" :style="{'background': item.result.rgba}"></div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.sample {
  width: 80px;
  height: 80px;
}
.group{
  padding: 3em;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-flex-direction: row;
  -ms-flex-direction: row;
  flex-direction: row;
  -webkit-flex-wrap: wrap;
  -ms-flex-wrap: wrap;
  flex-wrap: wrap;
  -webkit-justify-content: center;
  -ms-flex-pack: center;
  justify-content: center;
  -webkit-align-content: space-around;
  -ms-flex-line-pack: distribute;
  align-content: space-around;
  -webkit-align-items: flex-start;
  -ms-flex-align: start;
  align-items: flex-start;
}
.line{
  -webkit-order: 0;
    -ms-flex-order: 0;
    order: 0;
    -webkit-flex: 0 1 auto;
    -ms-flex: 0 1 auto;
    flex: 0 1 auto;
    -webkit-align-self: auto;
    -ms-flex-item-align: auto;
    align-self: auto;
}
</style>

<script>
import { Culture } from "color-culture";

export default {
  components: {},
  props: {
    value: Culture
  },
  data: function() {
    return {};
  },
  computed: {},
  methods: {}
};
</script>