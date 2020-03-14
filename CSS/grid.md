# CSS Grid

To define an items position withing a grid column, use the **grid-column-start** and **grid-column-end** properties (the end position is the number of the last grid-line). Start and end can also be used reversed (with the end position having a smaller value than the start position)  
If you want to count grid lines from the right instead of the left, you can give grid-column-start and grid-column-end negative values. For example, you can set it to -1 to specify the first grid line from the right.  
<br/>
Instead of defining a grid item based on the start and end positions of the grid lines, you can define it based on your desired column width using the **span** keyword. Keep in mind that span only works with positive values.  
For example: 
```CSS
#garden {
  display: grid;
  grid-template-columns: 20% 20% 20% 20% 20%;
  grid-template-rows: 20% 20% 20% 20% 20%;
}

#water {
  grid-column-start: 2;
  grid-column-end: span 2
}
```  
<br/>

**grid-column** is a shorthand property that can accept both values at once, separated by a slash.

**grid-row** works the same as grid-column, only vertically.  

**grid-area** accepts four values separated by slashes: grid-row-start, grid-column-start, grid-row-end, followed by grid-column-end.  
<br/>



If grid items aren't explicitly placed with grid-area, grid-column, grid-row, etc., they are automatically placed according to their order in the source code. We can override this using the order property, which is one of the advantages of grid over table-based layout.
By default, all grid items have an order of 0, but this can be set to any positive or negative value, similar to z-index.  
<br/>

grid-template-columns: repeat(5, 20%);  
  
grid-template-columns doesn't just accept values in percentages, but also length units like pixels and ems. You can even mix different units together.
```CSS
#garden {
  display: grid;

  grid-template-rows: 20% 20% 20% 20% 20%;
}
```  
<br>

Grid also introduces a new unit, the **fractional fr**. Each fr unit allocates one share of the available space. For example, if two elements are set to 1fr and 3fr respectively, the space is divided into 4 equal shares; the first element occupies 1/4 and the second element 3/4 of any leftover space.
When columns are set with pixels, percentages, or ems, any other columns set with fr will divvy up the space that's left over.  

**grid-template** is a shorthand property that combines grid-template-rows and grid-template-columns.
For example, grid-template: 50% 50% / 200px; will create a grid with two rows that are 50% each, and one column that is 200 pixels wide.


--------
source: https://cssgridgarden.com/