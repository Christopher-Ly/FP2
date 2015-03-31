# Final Project Assignment 2: Explore One More! (FP2) 
DUE March 30, 2015 Monday (2015-03-30)

This is just like FP1, but where you do a different library. (Full description of FP1 is [on Piazza.][piazza])

During this assignment, you should start looking for teammates. See the project schedule [on Piazza.][schedule]

Write your report right in this file. Instructions are below. You can delete them if you like, or just leave them at the bottom.
You are allowed to change/delete anything in this file to make it into your report. It will be public.

This file is formatted with the [**markdown** language][markdown], so take a glance at how that works.

This file IS your report for the assignment, including code and your story.

Code is super easy in markdown, which you can easily do inline `(require net/url)` or do in whole blocks:
```
#lang racket

(require net/url)
```

### My Library: (library name here)
Write what you did!
Remember that this report must include:
 
* a narrative of what you did
* the code that you wrote
* output from your code demonstrating what it produced
* any diagrams or figures explaining your work 
 
The narrative itself should be no longer than 350 words. Yes, you can add more files and link or refer to them. This is github, handling files is awesome and easy!

Ask questions publicly in the Piazza group.

### How to Do and Submit this assignment

1. To start, [**fork** this repository][forking].
1. Modify the README.md file and [**commit**][ref-commit] changes to complete your solution.
  2. (This assignment is just one README.md file, so you can edit it right in github without cloning)
  3. (You may need to clone and push if you want to add extra files)
1. [Create a **pull request**][pull-request] on the original repository to turn in the assignment.

<!-- Links -->
[piazza]: https://piazza.com/class/i55is8xqqwhmr?cid=411
[schedule]: https://piazza.com/class/i55is8xqqwhmr?cid=453
[markdown]: https://help.github.com/articles/markdown-basics/
[forking]: https://guides.github.com/activities/forking/
[ref-clone]: http://gitref.org/creating/#clone
[ref-commit]: http://gitref.org/basic/#commit
[ref-push]: http://gitref.org/remotes/#push
[pull-request]: https://help.github.com/articles/creating-a-pull-request
[clyde]: http://imgur.com/gKqKR8C
-----------------------------------------------------------------------------------------------------------------------------

#####Student: Christopher Ly
#####Professor: Mark Sherman
#####Course: OPL
#####Date: 3/30/2015

###Library Explored: images.rkt and universe.rkt both of which are from the How to Design Programs 2e Teachpacks.

##Racket Library Exploration 2:

 As you may recall, in my first exploration I wanted to not only display a graphic, but I wanted to manipulate it's movement. To achieve this goal I attempted to use the drawing toolkit library, but it was missing manipulation. I then discovered the How to Design Programs library and found the arrow.rkt library which allowed to self control the movement and direction of the shape. For my second exploration I wanted to continue to explore the movements of a graphic, however, this time I didn't want it's movement to be controlled and determined at run-time, but rather have it already pre-determined. 
 
 For this task I used the How to Design Programs (HtDP) library collection again to search for the proper libraries that could be of use. From the HtDP I discovered the images.rkt and universe.rkt libraries. I combined both libraries in order to accomplish a graphic movement simulation. 
 
 I needed the images.rkt library in order to incorporate a graphic that will be used to see movement. The great thing about this library is that it allowed me to insert any graphic of my choice from the web (I ended up picking clyde from pacman). The images.rkt library also has two useful functions place-image and empty-scene; both of which positions your graphic as instructed and creates the frame for which that image can be positioned on, respectively. 
 
 As for the universe.rkt library it provides an animate function which takes a procedure and applies that procedure for a continous amount of iterations until the program completes it's run or the user halts the program. The animate function allowed me, for each iteration, to re-position the image on the frame. I ended up writing a procedure that had the graphic move down in a straight line and once it hit the bottom of the frame (ex. a boundary or wall) the graphic turned around and started moving up to the top of the frame. 
 
 What I liked about how I implemented my procedure was that I made sure my graphic didn't go beyond the boundaries or frames. This will be very so I don't allow any graphics that I don't want to overlap. I also achieved the idea of giving a graphic pre-determined movements. This implementation adds a little bit of artificial intelligence techniques (AI), which will come in handy for the final project. I also tried to implement this procedure all in one procedure but had no success doing so I split it up into multiple procedures and found success.
 
 ####Tested Code:
 
 ```
;;The Language must be Beginning Student. In order to insert the image.

(require 2htdp/image)
(require 2htdp/universe)

;;Constants
(define HEIGHT 300)
(define WIDTH 300)
(define CLYDE .)

(define (create-clyde-down height)
  (cond
    [(<= height (- HEIGHT (/ (image-height CLYDE) 2)))
     (place-image CLYDE 150 height (empty-scene WIDTH HEIGHT))]
    [(> height (- HEIGHT (/ (image-height CLYDE) 2)))
     (place-image CLYDE 150 (- HEIGHT (/ (image-height CLYDE) 2))
                  (empty-scene WIDTH HEIGHT))]))

(define (create-clyde-up height)
  (cond
    [(<= height (- HEIGHT (/ (image-height CLYDE) 2)))
     (place-image CLYDE 150 (- HEIGHT height) (empty-scene WIDTH HEIGHT))]
    [(> height (- HEIGHT (/ (image-height CLYDE) 2)))
     (place-image CLYDE 150 (- HEIGHT (- HEIGHT (/ (image-height CLYDE) 2)))
                  (empty-scene WIDTH HEIGHT))]))

;; This function properly moves an image downward until it reaches it's end frame and then continues by having the image go all the way up until the top frame.
(define (create-clyde height)
  (cond ((< height HEIGHT)
         (cond
           [(<= height (- HEIGHT (/ (image-height CLYDE) 2)))
            (place-image CLYDE 150 height (empty-scene WIDTH HEIGHT))]
           [(> height (- HEIGHT (/ (image-height CLYDE) 2)))
            (place-image CLYDE 150 (- HEIGHT (/ (image-height CLYDE) 2))
                         (empty-scene WIDTH HEIGHT))]))
        (else (create-clyde-up (- height HEIGHT)))))

;; This function was my initial attempt to get the image to move up after it finished moving downward.  

(define (create-clyde-test height)
  (cond ((< height HEIGHT)
         (cond
           [(<= height (- HEIGHT (/ (image-height CLYDE) 2)))
            (place-image CLYDE 150 height (empty-scene WIDTH HEIGHT))]
           [(> height (- HEIGHT (/ (image-height CLYDE) 2)))
            (place-image CLYDE 150 (- HEIGHT (/ (image-height CLYDE) 2))
                         (empty-scene WIDTH HEIGHT))]))
        ((> height HEIGHT)
         (cond
           [(<= height (+ HEIGHT (- HEIGHT (/ (image-height CLYDE) 2))))
            (place-image CLYDE 150 (- HEIGHT (- height HEIGHT)) (empty-scene WIDTH HEIGHT))]
           [(> height (+ HEIGHT (- HEIGHT (/ (image-height CLYDE) 2))))
            (place-image CLYDE 150 (- HEIGHT (- HEIGHT (/ (image-height CLYDE) 2))) (empty-scene WIDTH HEIGHT))]))))

;;RUN the code.

(animate create-clyde)
```
####Output:

Graphic Movement Output URL: [clyde]

(This is just a still image of the running output. Output is meant to be interactive to see movement.)
