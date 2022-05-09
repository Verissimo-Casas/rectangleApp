<template>
  <v-stage
    ref="stage"
    :config="stageSize"
    @mousemove="handleMouseMove"
    @mouseDown="handleMouseDown"
    @mouseUp="handleMouseUp"
  >
    <v-layer ref="layer">
      <v-rect
        v-for="(rec, index) in recs"
        :key="index"
        :config="{
          x: Math.min(rec.startPointX, rec.startPointX + rec.width),
          y: Math.min(rec.startPointY, rec.startPointY + rec.height),
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

export default {
  data() {
    return {
      stageSize: {
        width: width,
        height: height,
      },
      lines: [],
      isDrawing: false,
      recs: [],
    };
  },
  methods: {
    setRecs(element) {
      this.recs = element;
    },
    handleMouseDown(event) {
      this.isDrawing = true;
      const pos = this.$refs.stage.getNode().getPointerPosition();
      this.setRecs([
        ...this.recs,
        { 
          startPointX: pos.x, 
          startPointY: pos.y, 
          width: 0, 
          height: 0 
        },
      ]);
    },
    handleMouseUp() {
      this.isDrawing = false;
    },
    handleMouseMove(event) {
      if (!this.isDrawing) {
        return;
      }
      const point = this.$refs.stage.getNode().getPointerPosition();

      let curRec = this.recs[this.recs.length - 1];
      curRec.width = point.x - curRec.startPointX;
      curRec.height = point.y - curRec.startPointY;
    },
  },
};
</script>