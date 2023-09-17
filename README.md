>
```
let img;

function preload() {
  // 이미지를 미리 로드
  img = loadImage('bird1.jpg');
}

function setup() {
  createCanvas(img.width * 2, img.height);
  
  // 이미지를 그림
  image(img, 0, 0);

  // 흑백 이미지로 변환
  img.filter(GRAY);

  // 흑백 이미지를 그림
  image(img, img.width, 0);

  // 히스토그램 스트래칭
  img.loadPixels();
  let minVal = 255;
  let maxVal = 0;
  for (let i = 0; i < img.pixels.length; i += 4) {
    let pixelValue = img.pixels[i];
    minVal = min(minVal, pixelValue);
    maxVal = max(maxVal, pixelValue);
  }
  for (let i = 0; i < img.pixels.length; i += 4) {
    img.pixels[i] = map(img.pixels[i], minVal, maxVal, 0, 255);
    img.pixels[i + 1] = img.pixels[i];
    img.pixels[i + 2] = img.pixels[i];
  }
  img.updatePixels();

  // 히스토그램 평탄화
  let hist = new Array(256).fill(0);
  for (let i = 0; i < img.pixels.length; i += 4) {
    let pixelValue = img.pixels[i];
    hist[pixelValue]++;
  }
  let sum = hist.reduce((acc, val) => acc + val, 0);
  let cumulative = 0;
  for (let i = 0; i < 256; i++) {
    cumulative += hist[i];
    let newValue = map(cumulative, 0, sum, 0, 255);
    hist[i] = newValue;
  }
  for (let i = 0; i < img.pixels.length; i += 4) {
    let pixelValue = img.pixels[i];
    img.pixels[i] = hist[pixelValue];
    img.pixels[i + 1] = hist[pixelValue];
    img.pixels[i + 2] = hist[pixelValue];
  }
  img.updatePixels();

  // 변환된 이미지를 그림
  image(img, 0, 0);
}
```
