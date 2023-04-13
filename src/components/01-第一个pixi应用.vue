<template>
  <div></div>
</template>

<script setup>
// 导入pixi.js
import * as PIXI from "pixi.js";

// 创建应用
const app = new PIXI.Application({
  width: window.innerWidth,
  height: window.innerHeight,
  backgroundColor: 0x1099bb,
  resolution: window.devicePixelRatio || 1,
});

// 将应用画布添加到DOM中
document.body.appendChild(app.view);

// 创建一个矩形
const rectangle = new PIXI.Graphics();
rectangle.beginFill(0x66ccff); // 填充颜色
rectangle.drawRect(200, 200, 164, 64); // 绘制矩形
rectangle.endFill(); // 结束填充

// 将矩形添加到舞台
app.stage.addChild(rectangle);
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
canvas {
  width: 100vw;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
}
</style>
