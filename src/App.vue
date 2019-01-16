<template>
  <div>
    <Box class="box" id="b1" :onValueChange="{ x : change }"/>
    <div class="zone" id="z1">TESTE TESTE TESTE TESTE</div>
    <p v-if="over">HOVERED</p>
  </div>
</template>

<script>
import posed from 'vue-pose';

export default {
  name: 'App',
  components: {
    Box: posed.div({
      draggable: true,
    }),
  },
  data() {
    return {
      over: false,
    };
  },
  methods: {
    change(x) {
      let e = document.getElementById('z1');
      let b = document.getElementById('b1');
      e = e.getBoundingClientRect();
      b = b.getBoundingClientRect();
      const eX = e.left;
      const eX2 = e.right;
      const bX = b.left + x;
      const bX2 = bX + b.width;
      console.log(eX, bX, eX2, bX2);
      this.over = (bX >= eX && bX2 <= eX2);
    },
  },
};
</script>

<style>
body {
  display: flex;
  height: 95vh;
  padding: 0;
  margin: 0;
  justify-content: center;
  align-items: center;
}

.box {
  width: 100px;
  height: 100px;
  background: #54e365;
}
</style>
