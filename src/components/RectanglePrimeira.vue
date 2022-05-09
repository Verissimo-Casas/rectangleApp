<template>
  <v-stage
    ref="stage"
    :config="stageSize"
    @click="handleClick"
    @mousemove="handleMouseMove"
  >
    <v-layer ref="layer">
      <v-rect
        v-for="(rec, index) in recs"
        :key="index"
        :config="{
          x: Math.min(rec.startPointX, rec.startPointX + rec.width),
          y: Math.min(rec.startPointY, rec.startPointY + rec.width),
          width: Math.abs(rec.width),
          height: Math.abs(rec.height),
          stroke: 'blue',
          strokeWidth: 3,
        }"
      />
    </v-layer>
  </v-stage>
</template>

<script>
const width = window.innerWidth;
const height = window.innerHeight;

export default{
  name: "rectangle",

  data() {
    return {
      stageSize: {
        width: width,
        height: height,
      },
      isDrawing: false,
      clickCounter: 0,
      recs: [], //
    };
  },

methods: {
    setRecs(element) {
      this.recs = element;
    },
    setLines(element) {
      this.lines = element;
    },
    handleClick(event) {
      this.clickCounter++;
      const pos = this.$refs.stage.getNode().getPointerPosition();
      if (this.clickCounter == 1) {
        this.setRecs([
          ...this.recs,
          {
            startPointX: pos.x,
            startPointY: pos.y,
            width: 0,
            height: 0,
            deepWidth: 0,
            deepHeight: 0,
          },
        ]);
      }
      if (this.clickCounter == 2) {
        this.clickCounter = 0;
        this.lineGenerator(pos);
      }
    },
    handleMouseMove(e) {
      const point = this.$refs.stage.getNode().getPointerPosition();
      let curRec = this.recs[this.recs.length - 1];

      if (this.clickCounter == 1) {
        curRec.width = point.x - curRec.startPointX;
        curRec.height = point.y - curRec.startPointY;
      }
    },
  },
}
</script>