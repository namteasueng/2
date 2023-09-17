>이번 과제를 진행하면서 영상처리 분야의 히스토그램에 대해서 알아보고 이를 코드로 구현하여 보았습니다. 직접 이미지를 입력하여 입력된 이미지와 출력된 이미지를 비교하여보니 흐릿한 이미지나 이미지의 밝기(혹은 이미지의 명암)가 큰 편차가 없는 이미지에서 히스토그램 스트래칭 및 평탄화로 완성된 이미지가 더욱 눈에 들어왔습니다. 이런 실습을 통하여 디지털영처리에 대해서 좀 더 이해한것 같아서 너무 기쁩니다. 물론 코드를 히스토그램 평탄화 과정을 코드로 구현하는 구간이 조금 어려웠지만 웹검색과 chat GPT의 도움을 받아서 결과적으로 완성되어 너무 즐겁습니다.

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
