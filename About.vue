<template>
  <div class="time-graph">
    <canvas id="time-graph-canvas" :width="canvasHeight" :height="canvasHeight"></canvas>
  </div>
  <div class="time-graph">
    <canvas id="weather-graph-canvas" :width="canvasWidth" :height="canvasHeight"></canvas>
  </div>
</template>

<script>
import logo from '@/assets/logo.png';

export default {
  data() {
    return {
      img: new Image(),
      canvasWidth: '0',
      canvasHeight: '0',
      len: 200,
    };
  },
  created() {
    this.img.src = logo;
    this.img.width = this.len;
    this.img.height = this.len;
    this.canvasWidth = this.len.toString();
    this.canvasHeight = this.len.toString();
  },
  mounted() {
    this.img.onload = () => {
      const time_canvas = document.getElementById('time-graph-canvas');
      this.drawMain(time_canvas, 70, '#85d824', 'transparent');

      const weather_canvas = document.getElementById('weather-graph-canvas');
      this.drawMain(weather_canvas, 90, '#2ba0fb', 'transparent');
    };
  },
  methods: {
    drawMain(drawing_elem, percent, forecolor, bgcolor) {
      /*
          @drawing_elem: 绘制对象
          @percent：绘制圆环百分比, 范围[0, 100]
          @forecolor: 绘制圆环的前景色，颜色代码
          @bgcolor: 绘制圆环的背景色，颜色代码
      */
      const context = drawing_elem.getContext('2d');
      const center_x = drawing_elem.width / 2; // 圆心的X坐标
      const center_y = drawing_elem.height / 2; // 圆心的Y坐标
      const rad = (Math.PI * 2) / 100;
      const lineWidth = 20; // 圆环的线宽
      let speed = 2;
      const that = this;

      // 绘制背景圆圈
      function backgroundCircle() {
        context.save();
        context.beginPath();
        context.lineWidth = lineWidth; // 设置线宽
        const radius = center_x - context.lineWidth;
        context.lineCap = 'round'; // 设置线头的类型 'round' 圆头  'square' 矩形头
        context.fillStyle = context.createPattern(that.changePictureSize(that.img, that.img.width, that.img.height), 'no-repeat'); // 在指定点绘制图像(原始大小)
        context.fillRect(0, 0, Number(that.img.width), Number(that.img.height));
        context.strokeStyle = bgcolor;
        context.arc(center_x, center_y, radius, 0, Math.PI * 2, false);
        context.stroke();
        context.fill();
        context.closePath();
        context.restore();
      }

      // 绘制运动圆环
      function foregroundCircle(n) {
        context.save();
        context.strokeStyle = forecolor;
        context.lineWidth = lineWidth;
        context.lineCap = 'round';
        const radius = center_x - context.lineWidth;
        context.beginPath();
        context.arc(center_x, center_y, radius, -Math.PI / 2, -Math.PI / 2 + n * rad, false); // 用于绘制圆弧context.arc(x坐标，y坐标，半径，起始角度，终止角度，顺时针/逆时针)
        context.stroke();
        context.closePath();
        context.restore();
      }

      // 绘制文字
      function text(n) {
        context.save(); // save和restore可以保证样式属性只运用于该段canvas元素
        context.fillStyle = forecolor;
        const font_size = 40;
        context.font = `${font_size}px Helvetica`;
        const text_width = context.measureText(`${n.toFixed(0)}%`).width;
        context.fillText(`${n.toFixed(0)}%`, center_x - text_width / 2, center_y + font_size / 2);
        context.restore();
      }

      // 执行动画
      (function drawFrame() {
        window.requestAnimationFrame(drawFrame);
        context.clearRect(0, 0, drawing_elem.width, drawing_elem.height);
        // pic();
        backgroundCircle();
        text(speed);
        foregroundCircle(speed);
        if (speed >= percent) return;
        speed += 1;
      }());
    },
    changePictureSize(imgObj, width, height) {
      /*
          @imgObj: html的img元素
          @width: 转换后的图片宽度
          @height:  转换后的图片长度
      */
      const canvas = document.createElement('canvas');
      canvas.width = imgObj.width;
      canvas.height = imgObj.height;
      canvas.getContext('2d').drawImage(imgObj, 0, 0, width, height);
      const image = new Image();
      image.src = canvas.toDataURL('image/png');
      return image;
    },
  },
};
</script>

<style>
.time-graph {
  padding-top: 20px;
  display:flex;
  display:-webkit-flex;
  justify-content: center;
  align-items: center;
}

#time-graph-canvas {
  width: 80px;
  height: 80px;
}
</style>
