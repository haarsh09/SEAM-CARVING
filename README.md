**Seam Carving**
Seam Carving for Content Aware Image Resizing and Object Removal

**Introduction**
The goal of this project is to perform content-aware image resizing for both reduction and expansion and image object removal with seam carving operator. This allows image to be resized without losing or distorting meaningful content from scaling. The code in this repository and technique described below is an implementation of the algorithm presented in Avidan and Shamir and Avidan, Rubinstein, and Shamir algorithms.

**Algorithm Overview**
**Seam Removal**
Calculate energy map:
Energy is calculated by sum the absolute value of the gradient in both x direction and y direction for all three channel (B, G, R). Energy map is a 2D image with the same dimension as input image.

**Build accumulated cost matrix using forward energy:**
This step is implemented with dynamic programming. The value of each pixel is equal to its corresponding value in the energy map added to the minimum new neighbor energy introduced by removing one of its three top neighbors (top-left, top-center, and top-right)

**Find and remove minimum seam from top to bottom edge:**
Backtracking from the bottom to the top edge of the accumulated cost matrix to find the minimum seam. All the pixels in each row after the pixel to be removed are shifted over one column to the left if it has index greater than the minimum seam.

Repeat step 1 - 3 until achieving targeting width

Rotate image and repeat step 1 - 4 for vertical resizing:

Rotating image 90 degree counter-clockwise and repeat the same steps to remove rows.

**Seam Insertion**
Seam insertion can be thought of as inversion of seam removal and insert new artificial pixels/seams into the image. We first perform seam removal for n seams on a duplicated input image and record all the coordinates in the same order when removing. Then, we insert new seams to original input image in the same order at the recorded coordinates location. The inserted argificial pixel values are derived from an average of left and right neighbors.

**Seam Removing and Insertion with Mask**
When generating energy map, the region protected by mask are weighted with a very high positive value. This guarantees that the minimum seam will NOT be routed through the masked region so that we can provent pixels at this region from removing or distorting because of seam insertion.

**Object Removal**
Remove object by seam removal
When generating energy map, the region protected by mask are weighted with a very high negative value. This guarantees that the minimum seam will be routed through the masked region. Seam removal is performed repeatly until masked region has been completely removed as stated above with one more step; the same minimum seam also has to be removed from the mask in order to get correct energy map for the next step.

**Seam insertion**
Insering seams to return the image back to it's original dimensions.
