# CSS Animations
## Keyframes

```css
#animation {
  animation-name: colorful;
  animation-duration: 3s;
}

@keyframes colorful {
  0% {
   background-color: blue;
  }
  100% {
    background-color: yellow;
  }
}
```
- The animation-fill-mode specifies the style applied to an element when the animation has finished. (e.g. forwards)
- When elements have a specified position such as fixed or relative, the CSS offset properties right, left, top, and bottom can be used in animation rules to create movement.
- Another animation property is the animation-iteration-count, which allows you to control how many times you would like to loop through the animation.

### animation timing
The animation-timing-function property controls how quickly an animated element changes over the duration of the animation. If the animation is a car moving from point A to point B in a given time (your animation-duration), the animation-timing-function says how the car accelerates and decelerates over the course of the drive.


## Transform  
translate(x, y)  
scale(x, y)  
rotate(deg)  
transform-origin: 0 0;  
transform-origin: left; - transforms from the middle of the left side  
transform-origin: left top; - same as 0 0  

## Transition  
property  
duration  
timing-function  
delay  