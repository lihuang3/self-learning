Opencv functions and commands

#### 1. BASICS 


#### 2. TOPICS

##### 2.1 Open an single image

```
# import the OpenCV package
import cv2

# load the image with imread()
imageSource = 'images/messi5.jpg'
image = cv2.imread(imageSource)

# display the image on screen with imshow()
# after checking that it loaded
if image is not None:
    cv2.imshow('image',image)
elif image is None:
    print ("Error loading image")

# wait time in milliseconds
# this is required to show the image
# 0 = wait indefinitely
k = cv2.waitKey(0)

# close the window
cv2.destroyAllWindows()
```

##### 2.2 Open multiple images
```
# import the necessary packages
import cv2
import os, os.path

#debug info OpenCV version
print ("OpenCV version: " + cv2.__version__)

#image path and valid extensions
imageDir = "images/" #specify your path here
image_path_list = []
valid_image_extensions = [".jpg", ".jpeg", ".png", ".tif", ".tiff"] #specify your vald extensions here
valid_image_extensions = [item.lower() for item in valid_image_extensions]

#create a list all files in directory and
#append files with a vaild extention to image_path_list
for file in os.listdir(imageDir):
    extension = os.path.splitext(file)[1]
    if extension.lower() not in valid_image_extensions:
        continue
    image_path_list.append(os.path.join(imageDir, file))

#loop through image_path_list to open each image
for imagePath in image_path_list:
    image = cv2.imread(imagePath)
    
    # display the image on screen with imshow()
    # after checking that it loaded
    if image is not None:
        cv2.imshow(imagePath, image)
    elif image is None:
        print ("Error loading: " + imagePath)
        #end this loop iteration and move on to next image
        continue
    
    # wait time in milliseconds
    # this is required to show the image
    # 0 = wait indefinitely
    # exit when escape key is pressed
    key = cv2.waitKey(0)
    if key == 27: # escape
        break

# close any open windows
cv2.destroyAllWindows()
```
