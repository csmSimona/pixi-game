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
  antialias: true, // 抗锯齿
});

// 将应用画布添加到DOM中
document.body.appendChild(app.view);

// 创建一个矩形
const rectangle = new PIXI.Graphics();
// 设置边框样式
rectangle.lineStyle(4, 0xff0000, 1); // 线宽, 线颜色, 透明度
rectangle.beginFill(0x66ccff, 0.9); // 填充颜色, 透明度
rectangle.drawRect(0, 0, 164, 64); // 绘制矩形
rectangle.endFill(); // 结束填充

// 图形的缩放
// rectangle.scale.set(2, 2);
// 图形的位移
rectangle.position.set(100, 100);
// 图形的锚点
// 这个报错  查了一下graphics没有anchor，只有pivot
// rectangle.anchor.set(82, 32);
// 图形的中心点
// rectangle.pivot.set(82, 32);

// 将矩形添加到舞台
app.stage.addChild(rectangle);

// 创建一个圆形
const circle = new PIXI.Graphics();
circle.beginFill(0x66ccff, 0.9);
circle.drawCircle(0, 0, 32);
circle.endFill();
circle.position.set(300, 300);
app.stage.addChild(circle);
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
