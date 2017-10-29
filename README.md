# Identifying Lane Lines with Gradients
## Udacity’s Self-Driving Car Engineer NanoDegree Project 2

My goal in this project was to successfully identify and label the lanes the car was driving on. 
Each frame of the video needed to be analyzed and returned with red lines over the lanes the vehicle was in.

This project was a bit challenging for me simply because I had to learn about Hough space and Canny edge detection. 
But I loved the challenge and experimenting with ways to improve it. Most of mine failed, or would fail outside of these test examples, but it was still fun to try them out.

Here’s how I did it in the end.

![](https://cdn-images-1.medium.com/max/1320/1*XmYrsc6SBBr68EgpPK-Z4A.png)

Each frame went through a pipeline that I created. 
I started with what you see above, and to make taking the gradient easier, I converted the image to grayscale.

![](https://cdn-images-1.medium.com/max/1320/1*jA4S53yhKWJa9xQHBxrkUA.png)

From there I apply gaussian smoothing. 
This essentially blurs the image a little bit so that ragged edges are smoothed out and clearer lines can be drawn.

![](https://cdn-images-1.medium.com/max/1320/1*W7Gm2QWXg6UkHcmKEZybRg.png)

This is where the real fun begins. To find where the lanes are we need to have some way of telling what’s a line and what’s not. 
Well this requires detecting edges in the image. So the way we do this we take the gradient of every pixel to determine where the greatest changes in color are in the picture. 
Then we create a new picture with those changes represented in white and everywhere else in black. 
This is all done with the Canny function of opencv.

![](https://cdn-images-1.medium.com/max/1320/1*38OpRdrkmPfpvXCxcTKTsw.png)

As you can see, the algorithm has identified the places of highest change in the image, and created a new image with only those portioned represented in white. 
But at this point there is a lot of information there that we don’t need. 
The mountains and other lanes aren’t relevant to our goal so lets eliminate any points that aren’t within a cone of interest where we normally find the lane.

![](https://cdn-images-1.medium.com/max/1320/1*Arn-y_JXElQxUdVUcB3kmg.png)

Great, now we just need to connect these points into lines. 
A hough transform is used here to basically connect these points into many possible lines. 
We then take those lines and separate them into left and right lane line based on whether they have a positive or negative slope. 
Then we take the average of those two lines and extend it down to the bottom of the image, which results in this.

![](https://cdn-images-1.medium.com/max/1320/1*5-zYRPXOYnoMBfgi3bgHqg.png)

And now all we have to do is overlay that onto our original image and we should have our final result!

![](https://cdn-images-1.medium.com/max/1320/1*N_XqVDkLrf1Lp--m22TGsw.png)
