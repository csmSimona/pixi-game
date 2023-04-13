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
  backgroundColor: 0xffffff,
  resolution: window.devicePixelRatio || 1,
  antialias: true, // 抗锯齿
});

// 将应用画布添加到DOM中
document.body.appendChild(app.view);
// 创建容器
const container = new PIXI.Container();
// 将容器添加到舞台
app.stage.addChild(container);

// 添加恐龙小游戏的精灵纹理  基础纹理
const baseTexture = PIXI.BaseTexture.from(
  require("./assets/textures/game.png")
);

const frameWidth = 88;
const frameHeight = 100;
// 恐龙跳跃精灵（直立）
const jumpTexture = new PIXI.Texture(
  baseTexture,
  new PIXI.Rectangle(1680, 0, 82, frameHeight)
);
const jumpSprite = new PIXI.Sprite(jumpTexture);
jumpSprite.x = 60;
jumpSprite.y = window.innerHeight - 50 - frameHeight;
jumpSprite.visible = false;
container.addChild(jumpSprite);

// 恐龙跑步动画
let runTextures = [];
for (let i = 0; i < 2; i++) {
  runTextures.push(
    new PIXI.Texture(
      baseTexture,
      new PIXI.Rectangle(1680 + (2 + i) * frameWidth, 0, 82, frameHeight)
    )
  );
}
const runAnimation = new PIXI.AnimatedSprite(runTextures); // 动画精灵
runAnimation.x = 60;
runAnimation.y = window.innerHeight - 50 - frameHeight;
runAnimation.animationSpeed = 0.1;
runAnimation.play();
runAnimation.visible = false;
container.addChild(runAnimation);

// 恐龙死亡动画;
let deadTextures = [];
for (let i = 0; i < 2; i++) {
  deadTextures.push(
    new PIXI.Texture(
      baseTexture,
      new PIXI.Rectangle(1680 + (0 + i) * frameWidth, 0, 82, frameHeight)
    )
  );
}
const deadAnimation = new PIXI.AnimatedSprite(deadTextures);
deadAnimation.x = 60;
deadAnimation.y = window.innerHeight - 50 - frameHeight;
deadAnimation.animationSpeed = 0.1;
deadAnimation.play();
deadAnimation.visible = false;
container.addChild(deadAnimation);

// 仙人掌精灵
const cactusTexture = new PIXI.Texture(
  baseTexture,
  new PIXI.Rectangle(515, 0, 30, 60)
);
const cactusSprite = new PIXI.Sprite(cactusTexture);
cactusSprite.x = window.innerWidth;
cactusSprite.y = window.innerHeight - 50 - 50;
container.addChild(cactusSprite);

// 地面精灵
const groundTexture = new PIXI.Texture(
  baseTexture,
  new PIXI.Rectangle(50, 100, 2300, 30)
);
// 设置纹理水平镜像重复
groundTexture.baseTexture.wrapMode = PIXI.WRAP_MODES.REPEAT;
const groundSprite = new PIXI.TilingSprite(groundTexture); // 平铺精灵
// groundSprite.visible = false;
groundSprite.width = window.innerWidth;
groundSprite.height = 30;
// 设置地面精灵的位置
groundSprite.position.set(0, window.innerHeight - 50);
container.addChild(groundSprite);

// 创建文字
let startText = new PIXI.Text("开始游戏", {
  fontSize: 30,
  fill: 0x333333,
  align: "center",
});
startText.x = app.screen.width / 2;
startText.y = app.screen.height / 2 - 30;
startText.anchor.set(0.5);
container.addChild(startText);
// startText.interactive = true;
startText.eventMode = "static"; // 7.2推荐用这个
startText.cursor = "pointer";

startText.on("click", () => {
  playGame();
});

let isGameing = false;

// 开始游戏
function playGame(e) {
  // 重置场景位置
  groundSprite.tilePosition.x = 0;
  cactusSprite.x = window.innerWidth;
  // 恐龙跑步动画;
  runAnimation.visible = true;
  // 恐龙死亡动画;
  deadAnimation.visible = false;
  // 恐龙跳跃精灵
  jumpSprite.visible = false;

  startText.text = "当前得分：" + score;
  isGameing = true;
}

let jumpVelocity = 20; // 跳跃速度
let score = 0;
app.ticker.add(() => {
  if (!isGameing) return;
  if (isGameing) {
    // 地面精灵
    groundSprite.tilePosition.x -= 10; // 平铺的图像的偏移量
    // 仙人掌精灵
    cactusSprite.x -= 10;
    if (cactusSprite.x <= -30) {
      cactusSprite.x = window.innerWidth;
      score++;
      startText.text = "当前得分：" + score;
    }
  }

  if (jumpSprite.visible) {
    jumpVelocity -= 1;
    jumpSprite.y -= jumpVelocity; // 速度先变小后变大
    if (jumpSprite.y >= window.innerHeight - 50 - frameHeight) {
      jumpSprite.y = window.innerHeight - 50 - frameHeight;
      jumpVelocity = 20;
      runAnimation.visible = true;
      jumpSprite.visible = false;
    }
  }

  // 判断跳跃精灵与仙人掌精灵是否碰撞
  if (hitTestRectangle(jumpSprite, cactusSprite)) {
    gameOver();
  }

  // 判断跳跃精灵与仙人掌精灵是否碰撞
  // if (
  //   jumpSprite.y > cactusSprite.y - 60 &&
  //   jumpSprite.x + 60 > cactusSprite.x &&
  //   jumpSprite.x - 60 < cactusSprite.x
  // ) {
  //   // 游戏结束
  //   gameOver();
  // }
});

// 游戏结束
function gameOver() {
  isGameing = false;
  deadAnimation.visible = true;
  runAnimation.visible = false;

  startText.visible = false;
  let endText = new PIXI.Text(`游戏结束，最后得分：${score}\n\n重新开始`, {
    fontSize: 30,
    fill: 0x333333,
    align: "center",
  });
  endText.x = app.screen.width / 2;
  endText.y = app.screen.height / 2 - 30;
  endText.anchor.set(0.5);
  container.addChild(endText);
  endText.eventMode = "static";
  endText.cursor = "pointer";
  endText.on("click", () => {
    // location.reload();
    startText.visible = true;
    endText.visible = false;
    score = 0;
    playGame();
  });
}

window.addEventListener("keydown", (e) => {
  if (isGameing && e.code === "Space") {
    runAnimation.visible = false;
    jumpSprite.visible = true;
  }
});

// 碰撞检测
function hitTestRectangle(r1, r2) {
  let hit = false,
    combinedHalfWidths,
    combinedHalfHeights,
    vx,
    vy;

  // Find the center points of each sprite
  r1.centerX = r1.x + r1.width / 2;
  r1.centerY = r1.y + r1.height / 2;
  r2.centerX = r2.x + r2.width / 2;
  r2.centerY = r2.y + r2.height / 2;

  // Find the half-widths and half-heights of each sprite
  r1.halfWidth = r1.width / 2;
  r1.halfHeight = r1.height / 2;
  r2.halfWidth = r2.width / 2;
  r2.halfHeight = r2.height / 2;

  // Calculate the distance vector between the sprites
  vx = r1.centerX - r2.centerX;
  vy = r1.centerY - r2.centerY;

  // Figure out the combined half-widths and half-heights
  combinedHalfWidths = r1.halfWidth + r2.halfWidth - 20;
  combinedHalfHeights = r1.halfHeight + r2.halfHeight - 20;

  // Check for a collision on the x axis
  if (Math.abs(vx) < combinedHalfWidths) {
    if (Math.abs(vy) < combinedHalfHeights) {
      hit = true;
    } else {
      hit = false;
    }
  } else {
    hit = false;
  }

  return hit;
}
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
