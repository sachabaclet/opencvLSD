# OpenCV.js with LineSegmentDetector (LSD)

A custom build of OpenCV.js that includes the **Line Segment Detector** algorithm, which is missing from the official prebuilt versions.

## Quick Start

### Basic Usage

```html
<script async src="opencv.js" onload="onOpenCvLoaded()"></script>
<script>
async function onOpenCvLoaded() {
    // IMPORTANT: Modern OpenCV.js exports a factory function
    // You must call it to get the actual cv object
    if (typeof cv === 'function') {
        cv = await cv();
    }
    
    // Now you can use cv.LineSegmentDetector
    const lsd = new cv.LineSegmentDetector();
    
    // Detect lines in a grayscale image
    const lines = new cv.Mat();
    lsd.detect(grayImage, lines);
    
    // Draw detected lines
    lsd.drawSegments(outputImage, lines);
    
    // Cleanup (important to prevent memory leaks!)
    lines.delete();
    lsd.delete();
}
</script>
```

### CDN Usage

```html
<script async src="https://cdn.jsdelivr.net/gh/sachabaclet/opencvlsd@main/opencv.js"></script>
```

## API Reference

### LineSegmentDetector

```javascript
// Create detector
const lsd = new cv.LineSegmentDetector();

// Detect line segments
// image: CV_8UC1 grayscale image
// lines: output Mat containing detected lines (Nx1 CV_32FC4)
lsd.detect(image, lines);

// Draw detected lines on an image
// image: color image to draw on
// lines: detected lines from detect()
lsd.drawSegments(image, lines);

// Compare two sets of line segments
lsd.compareSegments(size, lines1, lines2, image);
```

### Reading Detected Lines

Each detected line is stored as 4 floats: `[x1, y1, x2, y2]`

```javascript
for (let i = 0; i < lines.rows; i++) {
    const x1 = lines.floatAt(i, 0);
    const y1 = lines.floatAt(i, 1);
    const x2 = lines.floatAt(i, 2);
    const y2 = lines.floatAt(i, 3);
    console.log(`Line ${i}: (${x1}, ${y1}) to (${x2}, ${y2})`);
}
```

## Build Info

- **OpenCV Version:** 4.13 (latest)
- **Emscripten:** 3.1.25
- **Build Type:** WASM with SIMD
- **Included Modules:** All standard modules (core, imgproc, dnn, photo, video, objdetect, calib3d, features2d)

## Building from Source

```bash
git clone https://github.com/YOUR_USERNAME/opencvlsd.git
cd opencvlsd
bash build_with_lsd.sh
```

Requirements: Ubuntu 20.04+, ~2GB disk space

## Use Cases

- Document edge detection
- Card/rectangle detection
- Lane detection
- Architectural line analysis

## License

OpenCV is released under the Apache 2.0 license.
