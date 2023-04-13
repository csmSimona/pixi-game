<template>
  <div></div>
</template>

<script setup>
// 导入pixi.js
import * as PIXI from "pixi.js";
import { OutlineFilter, GlowFilter } from "pixi-filters";

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

// 创建一个纹理
const texture = PIXI.Texture.from(require("./assets/textures/mujian.png"));
// 创建一个精灵
const sprite = new PIXI.Sprite(texture);
// 设置精灵的位置
sprite.x = app.screen.width / 2;
sprite.y = app.screen.height / 2;

// 设置精灵的锚点
sprite.anchor.set(0.5);

// 将精灵添加到舞台
app.stage.addChild(sprite);

// 创建模糊滤镜
const blurFilter = new PIXI.BlurFilter();
// 设置模糊滤镜的模糊程度
blurFilter.blur = 20;
// 将模糊滤镜添加到精灵上
// sprite.filters = [blurFilter];

// // 监听鼠标是否进入精灵
// sprite.interactive = true;
// sprite.on("pointerover", () => {
//   // 设置模糊滤镜的模糊程度
//   blurFilter.blur = 0;
// });
// sprite.on("pointerout", () => {
//   // 设置模糊滤镜的模糊程度
//   blurFilter.blur = 20;
// });

//创建轮廓滤镜
const outlineFilter = new OutlineFilter(5, 0xffff00); //2为轮廓宽度，0x000000为轮廓颜色
// 创建发光滤镜
const glowFilter = new GlowFilter({
  distance: 50,
  outerStrength: 1,
  innerStrength: 0,
  color: 0xff0000,
  quality: 0.5,
});
//将轮廓滤镜添加到精灵上
sprite.filters = [outlineFilter, glowFilter];
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
