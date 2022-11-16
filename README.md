Javascript를 사용한 얼굴인식 시스템
=============================
개요
---
https://www.youtube.com/watch?v=AZ4PdALMqx0   
영상을 참고하여 Javascript 기반으로 얼굴을 인식하는 시스템을 구현해보고 소감을 적으시오.

설명
---
p5.js 웹에디터로는 사진을 학습하여 얼굴인식을 하는데 어려움이 있어 웹캠을 통한 실시간 얼굴인식 프로그램을 작성해 보았습니다.   
웹캠을 통해 사람의 얼굴을 인식하면 얼굴 위에 레터박스가 만들어지게 됩니다.   
머신러닝 라이브러리 ml5를 사용하기 위해 'index.html' 파일을 수정했습니다.

코드
---
- index.html
~~~js
<script src="https://unpkg.com/ml5@0.5.0/dist/ml5.min.js" type="text/javascript"></script> 추가
~~~

- sketch.js
~~~js
let faceapi;
let video;
let detections;
 
// by default all options are set to true
const detectionOptions = {
    withLandmarks: true,
    withDescriptors: false,
    minConfidence: 0.5,
    MODEL_URLS: {
        //Mobilenetv1Model: 'https://raw.githubusercontent.com/ml5js/ml5-data-and-models/main/models/faceapi/ssd_mobilenetv1_model-weights_manifest.json',
        FaceLandmarkModel: 'https://raw.githubusercontent.com/ml5js/ml5-data-and-models/main/models/faceapi/face_landmark_68_model-weights_manifest.json',
        //FaceLandmark68TinyNet: 'https://raw.githubusercontent.com/ml5js/ml5-data-and-models/main/models/faceapi/face_landmark_68_tiny_model-weights_manifest.json',
        //FaceRecognitionModel: 'https://raw.githubusercontent.com/ml5js/ml5-data-and-models/main/models/faceapi/face_recognition_model-weights_manifest.json',
    },
};
 
function setup() {
    createCanvas(360,270);
 
    // load up your video
    video = createCapture(VIDEO);
    video.size(width, height);
 
    // video.hide(); // Hide the video element, and just show the canvas
    faceapi = ml5.faceApi(video, detectionOptions, modelReady);
    textAlign(RIGHT);
 
}
 
 
function draw() {
 
}

function modelReady() {
    console.log("ready!");
    console.log(faceapi);
    faceapi.detect(gotResults);
}
 
 
function gotResults(err, result) {
    if (err) {
        console.log(err);
        return;
    }
 
    // console.log(result)
    detections = result;
 
    // background(220);
    background(255);
    image(video, 0, 0, width, height);
 
    if (detections) {
        if (detections.length > 0) {
            // console.log(detections)
          drawBox(detections);
        }
    }
 
    faceapi.detect(gotResults);
}


function drawBox(detections) {
    for (let i = 0; i < detections.length; i += 1) {
        const alignedRect = detections[i].alignedRect;
        const x = alignedRect._box._x;
        const y = alignedRect._box._y;
        const boxWidth = alignedRect._box._width;
        const boxHeight = alignedRect._box._height;

        noFill();
        stroke(161, 95, 251);
        strokeWeight(2);
        rect(x, y, boxWidth, boxHeight);
    }
}
~~~

실행결과
------

https://user-images.githubusercontent.com/105781767/202179430-3ff5ae8a-a49c-4911-81e4-ee14ef54e2dd.mp4

