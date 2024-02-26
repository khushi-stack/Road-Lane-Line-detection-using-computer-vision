# Steps on what has been done

1) Loading test images
2) First we have imported all the essential libraries
3) Now, we have define a function called as list_images , which will display a list of images in a single figure with matplotlib.
4)Cmap in Python: Colormaps or Cmap in Python is a very useful tool for data visualization. It supports grayscale images by automatically applying a gray color map if the image has only two dimensions. The function removes the tick marks on the axes and adjusts the subplot spacing for optimal display. Overall, it provides a convenient way to visualize multiple images in a compact and organized manner.
5)Read the test images 
6)Now next step is color images , Lane lines in the test images are in white and yellow. We need to choose the most suitable color space, that clearly highlights the lane lines. We will try to retain as much of the lane lines as possible, while blacking out most of the other stuff.
7)Apply color selection to RGB images to blackout everything except for white and yellow lane lines.RGB_color_selection that takes an RGB image as input. It applies color selection to the image, isolating white and yellow lane lines while blacking out the rest of the image. It does this by creating two masks, one for white and another for yellow, and then combines them into a single mask. Finally, it applies this combined mask to the input image, resulting in an image with only white and yellow lane lines visible.
8) Now we r applying color selection test_images
9)Now we r doing HSV color space . HSV is an alternative representation of the RGB color model. The HSV representation models the way colors mix together, with the saturation dimension resembling various shades of brightly colored paint, and the value dimension resembling the mixture of those paints with varying amounts of black or white.Convert RGB images to HSV.
10)convert_hsv that converts RGB images to the HSV (Hue, Saturation, Value) color space using the OpenCV library. It takes an RGB image as input and returns the corresponding HSV representation. The code then applies this conversion function to a list of input images (presumably named test_images) using the list(map(...)) construct, resulting in a list of images converted to the HSV color space.
11)Now we are applying color selection to the HSV images to blackout everything except for white and yellow lane lines.
12)HSV_color_selection that applies color selection to HSV (Hue, Saturation, Value) images, isolating white and yellow lane lines while blacking out the rest of the image. The function first converts the input RGB image to the HSV color space using a helper function called convert_hsv. It then creates two masks, one for white and another for yellow, by specifying HSV threshold values. These masks are combined into a single mask using a bitwise OR operation, and the resulting mask is applied to the input image using a bitwise AND operation. The function returns an image with only white and yellow lane lines visible in the HSV color space.
Now HSL color space , HSL is an alternative representation of the RGB color model. The HSL model attempts to resemble more perceptual color models such as NCS or Munsell, placing fully saturated colors around a circle at a lightness value of 1/2, where a lightness value of 0 or 1 is fully black or white, respectively.
convert_hsl that converts RGB images to the HSL (Hue, Saturation, Lightness) color space using the OpenCV library. It takes an RGB image as input and returns the corresponding HSL representation. The code then applies this conversion function to a list of input images (presumably named test_images) using the list(map(...)) construct, resulting in a list of images converted to the HSL color space.Apply color selection to the HSL images to blackout everything except for white and yellow lane lines.

13)HSL_color_selection that applies color selection to HSL (Hue, Saturation, Lightness) images, isolating white and yellow lane lines while blacking out the rest of the image. The function first converts the input RGB image to the HSL color space using a helper function called convert_hsl. It then creates two masks, one for white and another for yellow, by specifying HSL threshold values. These masks are combined into a single mask using a bitwise OR operation, and the resulting mask is applied to the input image using a bitwise AND operation. The function returns an image with only white and yellow lane lines visible in the HSL color space. HSL will produce clearest lane lines of all color space.
14) Now proceed towards canny edge detection.
15)The Canny edge detector is an edge detection operator that uses a multi-stage algorithm to detect a wide range of edges in images.
First step is grayscaling the images where The Canny edge detection algorithm measures the intensity gradients of each pixel. So, we need to convert the images into gray scale in order to detect edges.
Now converting images to grayscale.
Second step is Guassian smoothing.
Since all edge detection results are easily affected by image noise, it is essential to filter out the noise to prevent false detection caused by noise. To smooth the image, a Gaussian filter is applied to convolve with the image. This step will slightly smooth the image to reduce the effects of obvious noise on the edge detector.
Now we are applying guassian filter to input images.
Now next step is to apply canny edge detection.
The Process of Canny edge detection algorithm can be broken down to 5 different steps:

Find the intensity gradients of the image
Apply non-maximum suppression to get rid of spurious response to edge detection.
Apply double threshold to determine potential edges.
Track edge by hysteresis: Finalize the detection of edges by suppressing all the other edges that are weak and not connected to strong edges.
If an edge pixel’s gradient value is higher than the high threshold value, it is marked as a strong edge pixel. If an edge pixel’s gradient value is smaller than the high threshold value and larger than the low threshold value, it is marked as a weak edge pixel. If an edge pixel's value is smaller than the low threshold value, it will be suppressed. The two threshold values are empirically determined and their definition will depend on the content of a given input image.

The Canny Edge Detection algorithm is applied to the input image with the specified threshold values, and the resulting edge-detected image is returned as the output. This function is useful for highlighting edges and boundaries in an image.
Next step is we r finding the region of interest , We're interested in the area facing the camera, where the lane lines are found. So, we'll apply region masking to cut out everything else.region_selection function is designed to extract a region of interest from an input image. It initializes an empty mask matching the image dimensions and adapts the mask color based on whether the input image is in color or grayscale. It defines a polygon representing the region of interest, with the polygon's vertices dynamically calculated according to the image's size. The function then fills this polygon with the specified color and extracts the region of interest by performing a bitwise AND operation between the input image and the filled polygon mask. This function is commonly employed in computer vision applications to isolate specific areas of interest within an image for subsequent analysis or processing.

Now we are going to do Hough transformation, The Hough transform is a technique which can be used to isolate features of a particular shape within an image. I'll use it to detected the lane lines in selected_region_images.
Here we are going to determine and cut the region of interest in the input images.
Hough transform The function returns a list of lines found in the input image, represented as line segments specified by their endpoints. It is a crucial step in line and shape detection within images, commonly used in computer vision and image processing applications.
hough_lines contains the list of lines detected in the selected region. Now, we will draw these detected lines onto the original test_images.
Now we are going to draw lines onto the input image.

Draw line function creates a copy of the input image to ensure that the original image remains unaltered. It then iterates through the list of lines and draws each line on the copied image using the specified color and thickness. The resulting image, with the drawn lines, is returned. This function is often used in computer vision and image processing to visualize detected lines or features on images.
Next step is Averaging and extrapolating the lane lines
We have multiple lines detected for each lane line. We need to average all these lines and draw a single line for each lane line. We also need to extrapolate the lane lines to cover the full lane line length.


Now we are going to Find the slope and intercept of the left and right lanes of each image.
the left and right lines are separated based on their slopes, with positive slopes indicating lines on the right and negative slopes indicating lines on the left. For each line, it computes the slope, intercept, and length, ensuring that lines with vertical slopes (x1 == x2) are skipped. The lengths of the lines are used as weights to give more importance to longer lines in the averaging process.

The weighted averages of the slopes and intercepts are then calculated for both the left and right lanes. If no lines are found for a particular lane, the result is set to None. These average parameters represent the estimated characteristics of the left and right lanes, which can be used for further processing and visualization in applications like lane detection in autonomous driving systems.
Now converting the slope and intercept of each line into pixel points.


Creating the full length lines from the pixel points , draw this lines onto the input images. 
Done with processing with images and now going to proceed with videos images.
1)Process the input frame to detect lane lines.
2)Read input video stream and produce a video file with detected lane lines.
3)Now we will going to see the ouput based on video streaming.
