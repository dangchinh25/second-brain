
- Use BEM convention for naming CSS class
- The browser may have some default sytling for each HTML tag (e.g `<img/>, <button>` etc) => May need to reset those styling manually in CSS
```css
* {

  margin: 0;

  padding: 0;

}

  

button {

  border: initial;

  background-color: initial;

  color: inherit;

  font: inherit;

  outline: none;

}
```

- Most tag inherit font related styling from parent if not explicitly set, except for button => Will have to set manually
```css
button {

  color: inherit;

  font: inherit;

}
```

- We can also create our own animation
	- First we need to define the property keyframe of the animation
	- Then we can start apply and use it in our actual custom css
```css
@keyframes fade-in-from-top {

  0% {

    opacity: 0;

    transform: translateY(-50px);

  }

  

  100% {

    opacity: 1;

    transform: translateY(0px);

  }

}

.logo {

  align-self: center;

  margin-left: 20px;

  margin-right: 30px;

  animation: fade-in-from-top 0.5s;

}

  

.dd-toggle {

  color: rgba(255, 255, 255, 0.7);

  padding: 0 13px;

  cursor: pointer;

  transition: all 0.4s;

  animation: fade-in-from-top 0.5s;

}
```

![](https://i.imgur.com/hm7ZE2T.png)

- `align-items` and `justify-content` got switch when we change `flex-directions`
- CSS has class modifier and pseudo element => ?
- In order for the `top` and `left` (or `left` and `right`) to work, the selected parent element need to have `position: relative`
- `margin-*: auto` trick is only available for flexbox component => This mean that it will take the space between this element and the other element
- By default, the width attribute will not include border and padding => Use `box-sizing: border-box`
- To use CSS grid
	- We need to predefined before hand the number and size of rows and columns
	- Set the predefined layout using `grid-template-columns/rows`
	- Then for each component, we have to manually put the element in the correct cell in the grid 
		- Using `grid-column/row`
- Everything else is kinda similar with Flexbox
![](https://i.imgur.com/tTOBQUU.png)
