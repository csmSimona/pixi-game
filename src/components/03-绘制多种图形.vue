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
// 图形的旋转
// rectangle.rotation = 0.5;
// 图形的锚点
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

// 绘制圆角矩形
const roundedRectangle = new PIXI.Graphics();
roundedRectangle.beginFill(0x66ccff, 0.9);
roundedRectangle.drawRoundedRect(0, 0, 164, 64, 10); // 绘制圆角矩形,第一个参数是x坐标,第二个参数是y坐标,第三个参数是宽度,第四个参数是高度,第五个参数是圆角半径
roundedRectangle.endFill();
roundedRectangle.position.set(500, 500);
app.stage.addChild(roundedRectangle);

// 绘制椭圆
const ellipse = new PIXI.Graphics();
ellipse.beginFill(0x66ccff, 0.9);
ellipse.drawEllipse(0, 0, 164, 64); // 绘制椭圆,第一个参数是x坐标,第二个参数是y坐标,第三个参数是宽度,第四个参数是高度
ellipse.endFill();
ellipse.position.set(700, 700);
app.stage.addChild(ellipse);

// 绘制多边形
const polygon = new PIXI.Graphics();
polygon.beginFill(0x660000, 0.9);
polygon.drawPolygon([0, 0, 100, 0, 100, 100, 0, 100]); // 绘制多边形,参数是一个数组,数组中的每个元素是一个点的坐标,每两个元素是一个点的x坐标和y坐标
polygon.endFill();
polygon.position.set(50, 300);
app.stage.addChild(polygon);

// 绘制圆弧
const arc = new PIXI.Graphics();
arc.beginFill(0x660000, 0.9);

// 绘制圆弧,第一个参数是x坐标,第二个参数是y坐标,第三个参数是半径,第四个参数是起始角度,第五个参数是结束角度,第六个参数是是否逆时针
arc.arc(0, 0, 32, 0, Math.PI, false);
arc.endFill();
arc.position.set(300, 50);
app.stage.addChild(arc);

// 绘制线段
const line = new PIXI.Graphics();
line.lineStyle(4, 0xff0000, 1);
line.moveTo(0, 0); // 设置线段的起始点
line.lineTo(100, 100);
line.lineTo(200, 0);
line.position.set(500, 50);
app.stage.addChild(line);
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
