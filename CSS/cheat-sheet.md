# CSS Cheat Sheet
## Box Shadow
- horizontal offset
- vertical offset
- blur-radius
- spread-radius
- color value

## Positioning

### relative
When the position of an element is set to relative, it allows to specify how CSS should move it relative to its current position in the normal flow of the page. It pairs with the CSS offset properties of left or right, and top or bottom. These determine how many pixels, percentages, or ems the item should be moved away from where it is normally positioned. Changing an element's position to relative does not remove it from the normal flow - other elements around it still behave as if that item were in its default position.

### absolute
Absolute positioning locks the element in place relative to its parent container. Unlike the relative position, this removes the element from the normal flow of the document, so surrounding items will ignore it. The CSS offset properties (top or bottom and left or right) are used to adjust the position. Since the element will be locked relative to its closest positioned ancestor, if you forget to add a position rule to the parent item (this is typically done using position: relative;), the browser will keep looking up the chain and ultimately default to the body tag.

### floats
This positioning tool does not actually use position, but sets the float property of an element. Floating elements are removed from the normal flow of a document and pushed to either the left or right of their containing parent element. It's commonly used with the width property to specify how much horizontal space the floated element requires.

## Images  
### Centering
```css
img{
  display: block;
  margin: auto;
}

```

### Responsive Image

```css
img {
  max-width: 100%;
  display: block;
  height: auto;
}
```
The max-width property of 100% scales the image to fit the width of its container, but the image won't stretch wider than its original width. Setting the display property to block changes the image from an inline element (its default) to a block element on its own line. The height property of auto keeps the original aspect ratio of the image. 

## :before and :after
For the :before and :after selectors to function properly, they must have a defined content property. It usually has content such as a photo or text. When the :before and :after selectors add shapes, the content property is still required, but it's set to an empty string.