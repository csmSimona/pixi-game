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

// 显示hello world文字
const text = new PIXI.Text("Hello World", {
  fontFamily: "Arial",
  fontSize: 180,
  fill: 0xff0000,
  align: "center",
});

// 设置文字位置
text.x = app.screen.width / 2;
text.y = app.screen.height / 2;

// 设置文字锚点
text.anchor.set(0.5);
// app.stage.addChild(text);

// 创建一个精灵
const bunny = PIXI.Sprite.from(require("./assets/textures/bg.png"));
// 用精灵铺满整个屏幕
bunny.width = app.screen.width;
bunny.height = app.screen.height;

// 使用文字作为精灵的遮罩
bunny.mask = text;

// 用圆形做遮罩
// const circle = new PIXI.Graphics();
// circle.beginFill(0x0000ff);
// circle.drawCircle(app.screen.width / 2, app.screen.height / 2, 100);
// circle.endFill();
// bunny.mask = circle;

// 用图片做遮罩
// const mujian = PIXI.Sprite.from(require("./assets/textures/mujian.png"));
// bunny.mask = mujian;

app.stage.addChild(bunny);
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
