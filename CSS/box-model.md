# Box Model

## Block and inline boxes
If a box is defined as a **block**, it will behave in the following ways:

- The box will extend in the inline direction to fill the space available in its container. In most cases this means that the box will become as wide as its container, filling up 100% of the space available.
- The box will break onto a new line.
- The width and height properties are respected.
- Padding, margin and border will cause other elements to be pushed away from the box  

If a box has an outer display type of **inline**, then:

- The box will not break onto a new line.
- The width and height properties will not apply.
- Padding, margin and borders will apply but will not cause other inline boxes to move away from the box.

## Border-Box
If you want all of your elements to use the alternative box model, and this is a common choice among developers, set the box-sizing property on the <html> element, then set all other elements to inherit that value, as seen in the snippet below. If you want to understand the thinking behind this, see the [CSS Tricks article](https://css-tricks.com/inheriting-box-sizing-probably-slightly-better-best-practice/) on box-sizing.
```CSS
html {
  box-sizing: border-box;
}
*, *::before, *::after {
  box-sizing: inherit;
}
```  

## Margin collapsing
A key thing to understand about margins is the concept of margin collapsing. If you have two elements whose margins touch, and both margins are positive, those margins will combine to become one margin, which is the size of the largest individual margin. If one or both margins are negative, the amount of negative value will subtract from the total.  
There are a number of rules that dictate when margins do and do not collapse. For further information see the detailed page on [mastering margin collapsing](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing). The main thing to remember for now is that margin collapsing is a thing that happens. If you are creating space with margins and don't get the space you expect, this is probably what is happening.  

## inline-block
There is a special value of display, which provides a middle ground between inline and block. This is useful for situations where you do not want an item to break onto a new line, but do want it to respect width and height and avoid overlapping.  

An element with display: inline-block does a subset of the block things we already know about:
  - The width and height properties are respected.
  - padding, margin, and border will cause other elements to be pushed away from the box.  

It does not, however, break onto a new line, and will only become larger than its content if you explicitly add width and height properties.  

Where this can be useful is when you want to give a link to a larger hit area by adding padding. ```<a>``` is an inline element like ```<span>```; you can use display: inline-block to allow padding to be set on it, making it easier for a user to click the link.