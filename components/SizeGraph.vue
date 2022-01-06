<template>
  <div class="wrapper">
    <img src="/icon_256.png" alt="MCK" width="200" height="200" ref="logo" class="logo">
    <canvas class="canvas" ref="canvas" width="300" height="300"></canvas>
    <p class="subtext">% Smaller</p>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: 'SizeGraph',
  props: {
    percent: { required: true, type: Number },
  },
  mounted() {
    if (process.client) {
      const canvas: HTMLCanvasElement = this.$refs.canvas as HTMLCanvasElement;
      const logo = this.$refs.logo as HTMLElement | null

      const ctx = canvas.getContext('2d');
      if (!ctx) return;

      const width = ctx.canvas.width;
      const height = ctx.canvas.height;

      const full = 2 * Math.PI

      const x = width / 2
      const y = width / 2
      const radius = (width / 2) - 15

      const draw = (ctx: CanvasRenderingContext2D, percent: number) => {

        ctx.clearRect(0, 0, width, height);

        const start = Math.PI * -0.5;

        // Draw filled
        ctx.lineWidth = 5
        ctx.strokeStyle = '#4fc791';
        ctx.beginPath()
        const endAngle = start + (full * (percent / 100));
        ctx.arc(x, y, radius, start, endAngle)
        ctx.stroke()


        ctx.fillStyle = '#61e7ab';
        ctx.textAlign = "center"
        ctx.font = "18px monospace"
        ctx.fillText('↓ ' + percent.toFixed(1) + '%',
          x, height - 30)
      }

      let startTime = new Date().getTime()

      const animate = () => {
        const time = new Date().getTime()
        let passed = (time - (startTime)) / 2000;
        passed = passed > 0.5 ? 4 * Math.pow((passed - 1), 3) + 1 : 4 * Math.pow(passed, 3);
        const value = Math.min(this.percent, this.percent * passed)
        const scale = 1 - (value / 100)

        if (logo != null) {
          logo.style.transform = `translate(-50%, -50%) scale(${ scale }, ${ scale })`
        }
        draw(ctx, value)
        if (value >= this.percent) return
        requestAnimationFrame(animate)
      }

      animate()
    }
  }
})
</script>
<style scoped lang="scss">


.wrapper {
  position: relative;
  width: 300px;
  text-align: center;
  box-sizing: border-box;
  margin: 1rem auto;
}

.logo {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.canvas {
  z-index: 1;
}

.subtext {
  color: #666;
}
</style>
