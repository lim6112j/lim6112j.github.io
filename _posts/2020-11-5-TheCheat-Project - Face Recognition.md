---
layout: post
comments: true
categories: diary
---
# In-House Working Solution - face recognition (mac)
install go on mac - [install go](https://golang.org/doc/install)
## install dlib
* brew install pkg-config dlib
* go get -u github.com/Kagami/go-face

## install opencv
* brew install opencv
* go get -u -d gocv.io/x/gocv
* cd /usr/local/lib/pkgconfig && ln -s ../../Cellar/opencv/4.5.0_3/lib/pkgconfig/opencv4.pc opencv4.pc
* environment setting - bashrc, zshrc modify
```
# for gocv
export CGO_CXXFLAGS="--std=c++11"
export CGO_CPPFLAGS="-I/usr/local/Cellar/opencv/4.5.0_3/include"
export CGO_LDFLAGS="-L/usr/local/Cellar/opencv/4.5.0_3/lib -lopencv_stitching -lopencv_superres -lopencv_videostab -lopencv_aruco -lopencv_bgsegm -lopencv_bioinspired -lopencv_ccalib -lopencv_dnn_objdetect -lopencv_dpm -lopencv_face -lopencv_photo -lopencv_fuzzy -lopencv_hfs -lopencv_img_hash -lopencv_line_descriptor -lopencv_optflow -lopencv_reg -lopencv_rgbd -lopencv_saliency -lopencv_stereo -lopencv_structured_light -lopencv_phase_unwrapping -lopencv_surface_matching -lopencv_tracking -lopencv_datasets -lopencv_dnn -lopencv_plot -lopencv_xfeatures2d -lopencv_shape -lopencv_video -lopencv_ml -lopencv_ximgproc -lopencv_calib3d -lopencv_features2d -lopencv_highgui -lopencv_videoio -lopencv_flann -lopencv_xobjdetect -lopencv_imgcodecs -lopencv_objdetect -lopencv_xphoto -lopencv_imgproc -lopencv_core"
```

## QT binding install In GOPATH (global installation) mode
* export GO111MODULE=off; xcode-select --install; go get -v github.com/therecipe/qt/cmd/... && $(go env GOPATH)/bin/qtsetup test && $(go env GOPATH)/bin/qtsetup -test=false
## QT compile an application
In GOPATH (global installation) mode
(replace github.com/therecipe/examples/... with the project you want to compile)

export GO111MODULE=off; xcode-select --install; go get -v github.com/therecipe/qt/cmd/... && $(go env GOPATH)/bin/qtdeploy test desktop github.com/therecipe/examples/basic/widget

# opencv test
```
package main

import (
	"gocv.io/x/gocv"
)

func main() {
	webcam, _ := gocv.VideoCaptureDevice(0)
	window := gocv.NewWindow("Hello")
	img := gocv.NewMat()

	for {
		webcam.Read(&img)
		window.IMShow(img)
		window.WaitKey(1)
	}
}
```

# dlib test
```
package main

import (
"fmt"
"log"
"path/filepath"

"github.com/Kagami/go-face"
)

// Path to directory with models and test images. Here it's assumed it
// points to the <https://github.com/Kagami/go-face-testdata> clone.
const dataDir = "testdata"

var (
	modelsDir = filepath.Join(dataDir, "models")
	imagesDir = filepath.Join(dataDir, "images")
)

// This example shows the basic usage of the package: create an
// recognizer, recognize faces, classify them using few known ones.
func main() {
	// Init the recognizer.
	rec, err := face.NewRecognizer(modelsDir)
	if err != nil {
		log.Fatalf("Can't init face recognizer: %v", err)
	}
	// Free the resources when you're finished.
	defer rec.Close()

	// Test image with 10 faces.
	testImagePristin := filepath.Join(imagesDir, "pristin.jpg")
	// Recognize faces on that image.
	faces, err := rec.RecognizeFile(testImagePristin)
	if err != nil {
		log.Fatalf("Can't recognize: %v", err)
	}
	if len(faces) != 10 {
		log.Fatalf("Wrong number of faces")
	}

	// Fill known samples. In the real world you would use a lot of images
	// for each person to get better classification results but in our
	// example we just get them from one big image.
	var samples []face.Descriptor
	var cats []int32
	for i, f := range faces {
		samples = append(samples, f.Descriptor)
		// Each face is unique on that image so goes to its own category.
		cats = append(cats, int32(i))
	}
	// Name the categories, i.e. people on the image.
	labels := []string{
		"Sungyeon", "Yehana", "Roa", "Eunwoo", "Xiyeon",
		"Kyulkyung", "Nayoung", "Rena", "Kyla", "Yuha",
	}
	// Pass samples to the recognizer.
	rec.SetSamples(samples, cats)

	// Now let's try to classify some not yet known image.
	testImageNayoung := filepath.Join(imagesDir, "nayoung.jpg")
	nayoungFace, err := rec.RecognizeSingleFile(testImageNayoung)
	if err != nil {
		log.Fatalf("Can't recognize: %v", err)
	}
	if nayoungFace == nil {
		log.Fatalf("Not a single face on the image")
	}
	catID := rec.Classify(nayoungFace.Descriptor)
	if catID < 0 {
		log.Fatalf("Can't classify")
	}
	// Finally print the classified label. It should be "Nayoung".
	fmt.Println(labels[catID])
}
```
