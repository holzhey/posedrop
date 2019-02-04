<template>
  <div>
    <Box class="box" :onValueChange="{ x : changeX, y: changeY }"/>
    <div class="zone">TESTE 1</div>
    <div class="zone">TESTE 2</div>
    <div class="zone">TESTE 3</div>
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
      over: null,
      x: 0,
      y: 0,
    };
  },
  methods: {
    changeY(y) {
      this.y = y + 50;
      this.checkBellow();
    },
    changeX(x) {
      this.x = x + 50;
      this.checkBellow();
    },
    getDistanceTo(htmlElement) {
      const elementCenterX = htmlElement.offsetLeft + (htmlElement.clientWidth / 2);
      const elementCenterY = htmlElement.offsetTop + (htmlElement.clientHeight / 2);
      const radius = Math.sqrt(
        ((this.x - elementCenterX) ** 2) + ((this.y - elementCenterY) ** 2)
      );
      return {
        radius: radius,
        element: htmlElement,
      };
    },
    checkBellow() {
      const zones = document.getElementsByClassName('zone');
      if (zones) {
        const zonesRadius = Array.prototype.map.call(zones, this.getDistanceTo);
        const zone = zonesRadius.filter(item => item.radius < 200)
          .sort((a, b) => {
            return b.radius - a.radius;
          })
          .pop();
        if (zone && zone.element !== this.over) {
          if (zone && zone.element) {
            zone.element.style.background = 'darkorange';
          }
          if (this.over) {
            this.over.style.background = 'aliceblue';
          }
          this.over = zone.element;
        }
      }
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
  position: absolute;
  top: 0px;
  left: 0px;
}

.zone {
  width: 220px;
  height: 220px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  border: 2px solid;
  border-color: rgb(253, 255, 73);
  text-align: center;
  background: aliceblue;
  transition: all 0.3s ease-in-out;
}
</style>
