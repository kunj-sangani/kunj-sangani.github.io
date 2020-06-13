---
date: 2020-06-13 09:45:26
layout: post
title: "Scan Barcode In SPFx WebPart"
subtitle:
description:
image:
optimized_image: /assets/img/Blogs/Blog3/BARCODE.jpg
category:
tags:
  - SharePoint
  - SPFx
author:
paginate: false
---

# Overview

This blog will help you use Quagga.js in the SPFx webpart to scan barcodes and retrieve barcode data.

Quagga.js works with all modern browsers except IE 11.
 
Quagga.js helps us to scan the barcode from two different sources:

1. Static Image
2. Live Stream

For more details on Quagga.js check out this link.
 
For the below demo we would use the Live Stream source which would use our device's camera to scan the barcode
 
To download the Code,
BarcodeScannerSPFx

# Installation

To install Quagga.js in SPFx solution please use the below command

```js
npm install quagga 
```

Import the reference of Quagga.js in the file where we need to scan the barcode.

```js
import Quagga from 'quagga'; 
```

If you are using React.js as the framework add the below code in the render method, otherwise if you are using any other framework add in the html section.

```js
<div id="liveStreamElement"></div>  
```

Use the below code to initialize and start the scan. This would start the camera and start the scanning of the barcode

```js
Quagga.init({  
      inputStream: {  
        name: "Live",  
        type: "LiveStream",  
        target: document.querySelector('#liveStreamElement'),  
        constraints: {  
          width: 320,  
          height: 240,  
          facingMode: "environment"  
        }  
      },  
      locator: {  
        patchSize: "x-large",  
        halfSample: true  
      },  
      locate: true,  
      decoder: {  
        readers: ["code_128_reader"]  
      }  
    }, (err) => {  
      if (err) {  
        return true;  
      }  
      console.log("Initialization finished. Ready to start");  
      Quagga.start();  
    }); 
```

#### <ins>Note</ins>
We can change the decoder reader's values -- currently the decoding is allowed for the below formats:

EAN, CODE 128, CODE 39, EAN 8, UPC-A, UPC-C, I2of5, 2of5, CODE 93 and CODABAR.
 
For displaying the borders when we detect the barcode use the below code:

```js
Quagga.onProcessed((result) => {  
        var drawingCtx = Quagga.canvas.ctx.overlay,  
          drawingCanvas = Quagga.canvas.dom.overlay;  
        if (result) {  
          if (result.boxes) {  
            drawingCtx.clearRect(0, 0, parseInt(drawingCanvas.getAttribute("width")), parseInt(drawingCanvas.getAttribute("height")));  
            result.boxes.filter((box) => {  
              return box !== result.box;  
            }).forEach((box) => {  
              Quagga.ImageDebug.drawPath(box, { x: 0, y: 1 }, drawingCtx, { color: "green", lineWidth: 2 });  
            });  
          }  
          if (result.box) {  
            Quagga.ImageDebug.drawPath(result.box, { x: 0, y: 1 }, drawingCtx, { color: "#00F", lineWidth: 2 });  
          }  
          if (result.codeResult && result.codeResult.code) {  
            Quagga.ImageDebug.drawPath(result.line, { x: 'x', y: 'y' }, drawingCtx, { color: 'red', lineWidth: 3 });  
          }  
        }  
      });   
```

Once the barcode is detected, to get the result use the below method:

```js
Quagga.onDetected((val) => {  
      alert(val.codeResult.code);  
    }); 
```

To stop the scanning use the below code

```js
Quagga.stop(); 
```

# Demo

![Demo](/assets/img/Blogs/Blog3/Demo.gif)