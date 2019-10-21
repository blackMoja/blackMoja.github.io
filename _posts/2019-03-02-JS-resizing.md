---
title: 이미지 리사이징
date: 2019-03-02
categories:
    - javascript
tags:
    - javascript
---
이미지 리사이징 로직
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/javascript.png)
<!--more-->

** ES5 **
```javascript
/**
 * resizeImage
 * 이미지 업로드 시 리사이즈(사이즈 축소)
 * @param {Object} settings 
 * @param {function} callback 
 */
function resizeImage(settings, callback) {
    var option = settings;
    var reader = new FileReader();
    var image = new Image(); // 이미지 엘리먼트 생성
    var canvas = document.createElement('canvas');

    // 리사이즈 된 canvas데이터(string형태) 받아서 Blob객체 인스턴스로 반환
    function dataURItoBlob(dataURI) {
        var bytes = dataURI.split(',')[0].indexOf('base64') >= 0 ?
            atob(dataURI.split(',')[1]) :
            encodeURI(dataURI.split(',')[1]);
        var mime = dataURI.split(',')[0].split(':')[1].split(';')[0];
        var max = bytes.length;
        var ia = new Uint8Array(max);
        for (var i = 0; i < max; i++) {
            ia[i] = bytes.charCodeAt(i);
        }

        return callback(new Blob([ia], {
            type: mime
        }));
    }

    // canvas 이용한 resize 수행 후 결과물을 dataURItoBlob에 전달하면서 호출
    function resize() {
        var width = 600;
        var height = 600;
        console.log('options original file : ', option.file.size);
        canvas.width = width;
        canvas.height = height;
        canvas.getContext('2d').drawImage(image, 0, 0, width, height);
        var dataUrl,
            q = 0.5,
            d = 0.5,
            size = Math.min(option.file.size, 300 * 1024), // 오리지널 파일사이즈를 넘지 않아요
            // size = 300 * 1024,
            encLength;

        // 7번을 돌려서 가장 최적의 사이즈를 찾아냄.
        for (var i = 0; i < 7; ++i) {
            // q만큼 줄이고 늘림
            dataUrl = canvas.toDataURL('image/jpeg', q);
            // 헤더를 제외한 나머지 순수 dataUrl length
            encLength = atob(dataUrl.replace('data:image/jpeg;base64,', '')).length;
            // 2의 n승의 수만큼 더하고 뺌
            if (encLength > size) {
                q -= d /= 2;
            } else {
                q += d /= 2;
            }
            console.log(encLength, q);
        }

        return dataURItoBlob(dataUrl);
    }

    return function () {
        reader.onload = function (readerEvent) { // 읽기 동작이 성공적으로 완료 되었을 때(1-2)
            image.onload = function () {
                resize(); // 읽기 성공 시 resize() 수행 (2-2)   
            };
            image.src = readerEvent.target.result; // src설정 (2-1)
        };
        reader.readAsDataURL(option.file); // 전달된 file을 읽는다 (1-1)
    }(this);
}
```

** ES6 **
```javascript
    /**
     * 이미지 업로드 시 리사이즈(사이즈 축소)
     * @param settings
     */
    resizeImage(settings: any) {
        // 와이드 맥스,height 맥스
        const { file, maxSize } = settings;

        const reader = new FileReader();
        const image = new Image(); // 이미지 엘리먼트 생성
        const canvas = document.createElement('canvas');


        // 리사이즈 된 canvas데이터(string형태) 받아서 Blob객체 인스턴스로 반환
        const dataURItoBlob = (dataURI: string) => {
            const bytes = dataURI.split(',')[0].indexOf('base64') >= 0 ?
                atob(dataURI.split(',')[1]) :
                encodeURI(dataURI.split(',')[1]);
            const mime = dataURI.split(',')[0].split(':')[1].split(';')[0];
            const max = bytes.length;
            const ia = new Uint8Array(max);
            for (let i = 0; i < max; i++) { ia[i] = bytes.charCodeAt(i); }
            return new Blob([ia], { type: mime });
        };


        // canvas 이용한 resize 수행 후 결과물을 dataURItoBlob에 전달하면서 호출
        const resize = () => {
            const width = 600;
            const height = 600;
            console.log('options original file : ', option.file.size);
            canvas.width = width;
            canvas.height = height;
            canvas.getContext('2d').drawImage(image, 0, 0, width, height);
            const dataUrl,
                q = 0.5,
                d = 0.5,
                size = Math.min(option.file.size, 300 * 1024), // 오리지널 파일사이즈를 넘지 않아요
                // size = 300 * 1024,
                encLength;

            // 7번을 돌려서 가장 최적의 사이즈를 찾아냄.
            for (let i = 0; i < 7; ++i) {
                // q만큼 줄이고 늘림
                dataUrl = canvas.toDataURL('image/jpeg', q);
                // 헤더를 제외한 나머지 순수 dataUrl length
                encLength = atob(dataUrl.replace('data:image/jpeg;base64,', '')).length;
                // 2의 n승의 수만큼 더하고 뺌
                if (encLength > size) {
                    q -= d /= 2;
                } else {
                    q += d /= 2;
                }
                console.log(encLength, q);
            }

            return dataURItoBlob(dataUrl);
        };


        return new Promise((ok, no) => {
            // 파일이 로딩되었을때
            reader.onload = (readerEvent: any) => { // 읽기 동작이 성공적으로 완료 되었을 때(1-2)
                image.onload = () => ok(resize()); // 읽기 성공 시 resize() 수행 (2-2)
                image.src = readerEvent.target.result; // src설정 (2-1)
            };
            reader.readAsDataURL(file); // 전달된 file을 읽는다 (1-1)
        });
    }
```