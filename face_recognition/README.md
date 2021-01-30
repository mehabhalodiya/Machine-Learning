## Face Recognition — Step by Step

### Step 1: Finding all the Faces

To find faces in an image, we’ll start by making our image black and white because we don’t need color data to find faces. Then we’ll look at every single pixel in our image one at a time.

For every single pixel, we want to look at the pixels that directly surrounding it. Our goal is to figure out how dark the current pixel is compared to the pixels directly surrounding it. Then we want to draw an arrow showing in which direction the image is getting darker:

![demo](https://miro.medium.com/max/625/1*WF54tQnH1Hgpoqk-Vtf9Lg.gif)

If you repeat that process for every single pixel in the image, you end up with every pixel being replaced by an arrow. These arrows are called gradients and they show the flow from light to dark across the entire image.

It would be better if we could just see the basic flow of lightness/darkness at a higher level so we could see the basic pattern of the image.
To do this, we’ll break up the image into small squares of 16x16 pixels each. In each square, we’ll count up how many gradients point in each major direction (how many point up, point up-right, point right, etc…). Then we’ll replace that square in the image with the arrow directions that were the strongest.

The end result is we turn the original image into a very simple representation that captures the basic structure of a face in a simple way:

![demo](https://miro.medium.com/max/875/1*uHisafuUw0FOsoZA992Jdg.gif)

To find faces in this HOG image, all we have to do is find the part of our image that looks the most similar to a known HOG pattern that was extracted from a bunch of other training faces:

![demo](https://miro.medium.com/max/875/1*6xgev0r-qn4oR88FrW6fiA.png)

### Step 2: Posing and Projecting Faces

Now we have to deal with the problem that faces turned different directions look totally different to a computer:

![demo](https://miro.medium.com/max/875/1*x-rg0aSpKOer1JF-TejYUg.png)

We will try to warp each picture so that the eyes and lips are always in the sample place in the image. To do this, we are going to use an algorithm called **face landmark estimation**.

![demo](https://miro.medium.com/max/875/1*igEzGcFn-tjZb94j15tCNA.png)

Now no matter how the face is turned, we are able to center the eyes and mouth are in roughly the same position in the image. This will make our next step a lot more accurate.

### Step 3: Encoding Faces

What we need is a way to extract a few basic measurements from each face. Then we could measure our unknown face the same way and find the known face with the closest measurements. For example, we might measure the size of each ear, the spacing between the eyes, the length of the nose, etc.

![demo](https://miro.medium.com/max/875/1*n1R8VMyDRw3RNO3JULYBpQ.png)

### Step 4: Finding the person’s name from the encoding

All we have to do is find the person in our database of known people who has the closest measurements to our test image (for now, I have just added sample images for testing purpose).

You can do that by using any basic machine learning classification algorithm. We’ll use a simple linear SVM classifier, but lots of classification algorithms could work.

All we need to do is train a classifier that can take in the measurements from a new test image and tells which known person is the closest match. Running this classifier takes milliseconds. The result of the classifier is the name of the person!