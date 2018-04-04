# find_lane_lines
Car to detect steps are as follows:
lane_find.py is my project code and the test_on_my_camera.py is the project code on my cemera.
1 lane_find.py:

(1) Applies the Canny transform

    low_threshold = 50
    high_threshold = 150
    cnny = canny(blr_gry, low_threshold, high_threshold)

(2)the region of image
    
    vertices = np.array([[(0,imshp[0]),(480, 315), (490, 315), (imshp[1],imshp[0])]], dtype=np.int32)
    mskd_edgs = region_of_interest(cnny, vertices)

(3)Applies an image mask.
    Only keeps the region of the image defined by the polygon  formed from vertices. The rest of the image is set to black and the region of image.

(4)we can extract yellow and white in HSV color space:

 def color_select(img):
  hsv = cv2.cvtColor(img, cv2.COLOR_RGB2HSV)
  lower_yellow = np.array([10, 110, 110])
  upper_yellow = np.array([20, 225, 225])
  yellow_mask = cv2.inRange(hsv, lower_yellow, upper_yellow)
  lower_white = np.array([0, 0, 215])
  upper_white = np.array([170, 30, 235])
  white_mask = cv2.inRange(hsv, lower_white, upper_white)
  color_mask = cv2.bitwise_or(yellow_mask, white_mask)
  gray = grayscale(img)
  darken = (gray / 3).astype(np.uint8)
  color_masked = cv2.bitwise_or(darken, color_mask)

(5)Use least squares to fit lane lines:
    this is the function you might want to use as a starting point once you want to average/extrapolate the line segments you detect to map out the full extent of the lane.
    Think about things like separating line segments by their slope ((y2-y1)/(x2-x1)) to decide which segments are part of the left line vs. the right line.  Then, you can average the position of each of the lines and extrapolate to the top and bottom of the lane.
    
   This function draws `lines` with `color` and `thickness`.  Lines are drawn on the image inplace (mutates the image).If you want to make the lines semi-transparent, think about combining this function with the weighted_img() function below



2 test_on_my_camera.py

note:We need to calibrate the camera and change the area of interest, because the camera is very bad in the first few frames, so we ignore the first few frames
