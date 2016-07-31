Deprecated. See [sass-tools](https://github.com/anthonyesau/sass-tools) for a refined system of tools for Sass.

#Sass Framework
A set of components to frame a new website style around.

##1. Sliding Scale

Utilizes a series of incremental media queries to vary the relative sizes of typography and other elements based on screen width.


###Features
- Unit agnostic (user defines the units)
- Ranges for the scale to slide along may include two or more values
- Fixed scale tools also included


###Fixed Scale Usage:

Though this component focuses on a responsive sliding scale, it also easily establishes a fixed scale. These protocols are oriented around typography so the base unit is referred to as ```$type-base``` which you can set to your preference. The ```$exponential-scale``` is the ratio of the each value in the scale to the values above or below it.

```scss
$type-base: 16px;
$exponential-scale: 1.618;
```

To access the values in your exponential scale, deploy the ```es()``` function. The argument (in number form) that you pass to it signifies the step in the exponential scale that is returned.

So if you use the same settings as these usage demonstration, the code ```es(0)``` returns the base value of ```16px```. A 3 would signify the 3rd step up on the exponential scale, while a -1 would be a step down.


###Sliding Scale Usage:
Define these ranges from minimum to maximum value:

```scss
$type-base-range: 100% 131.25%;
$exponential-scale-range: 1.333 1.618;
$screen-range: 20em 80em;
```

Some other settings:

```scss
//Operational settings
$increments: 2; //if unitless {number of steps (media queries) to calculate sliding changes}
//if units included {use distance for increments instead, must be same unit as screen-range}
$adjust-root: true; //True{change root(html) font-size to base}//false{leave root font-size at default}
$prefered-unit: 1rem; //Probably use em or rem
```

Implement it like this:
```scss
.foo {
  @include scale() { //initiate responsive scale for the contained properties
    font-size: es(0); //base value from the exponential scale
    line-height: ls(1.2 2); //linear interpolation of the list
    left: es(1) + 5rem; //add first step up on exponential scale to constant value
  }
}
```

And the css that is output:
```css
html {
  font-size: 100%;
}

html {
  font-size: 115.625%;
}

html {
  font-size: 131.25%;
}

@media (max-width: 20em) {
  .foo {
    font-size: 1rem;
    line-height: 1.2;
    left: 6.333rem;
  }
}
@media (min-width: 20em) and (max-width: 50em) {
  .foo {
    font-size: 1rem;
    line-height: 1.2;
    left: 6.333rem;
  }
}
@media (min-width: 50em) and (max-width: 80em) {
  .foo {
    font-size: 1rem;
    line-height: 1.6;
    left: 6.4755rem;
  }
}
@media (min-width: 80em) {
  .foo {
    font-size: 1rem;
    line-height: 2;
    left: 6.618rem;
  }
}
```

###TODO:
- create an option to use the calc() method of defining properties rather than a series of media queries.
- change linear scale to do the same thing as the exponential scale except in a linear fashion
- rename the old linear scale to linear interpolation
- figure out how to override default variable settings
- refine documentation


##2. Color Management

The color component is allows you to easily set multiple palettes and refer to colors within them.

Create some palettes like this. The palettes have individual names. Colors are named by the way they are used across multiple palettes.

```scss
$color-palettes: (
  light: (fg: #333, bg: #f8f8f8, accent: #ba9f61, fg-subtle: #808080, bg-subtle: #e5e5e5),
  dark: (fg: #f8f8f8, bg: #333, accent: #ba9f61, fg-subtle: #ccc, bg-subtle: #808080 ),
  accent: (fg: #f8f8f8, bg: #ba9f61, accent: #333, fg-subtle: #808080, bg-subtle: #ccc),
);
```

The first palette defined is set as the default, but you can change it to whatever you want:

```scss
@include set-palette-default(dark); // sets the dark palette as default
@include set-palette-default(3); // sets the 3rd palette (accent) as default
@include set-palette-default((fg: #f8f8f8, bg: #333, accent: #ba9f61, fg-subtle: #ccc, bg-subtle: #808080 )); //sets the map of colors provided as the default palette
```

Then access a color like this:
```scss
.foo {
  color: color(bg);
}
```

Which will render to css like this:
```css
.foo {
  color: #f8f8f8;
}
```

To define a property color value within multiple color palettes:
```scss
.accentline {
  @include color {
      border: 1px solid color(accent); //for each palette, this will always be the accent color
  }
}
```

It doesn't matter if the mixin is called on the outside of the selector. This renders like this:
```css
.primary-palette .accentline {
  border: 1px solid #ba9f61;
}
.light-palette .accentline {
  border: 1px solid #ba9f61;
}
.dark-palette .accentline {
  border: 1px solid #ba9f61;
}
.accent-palette .accentline {
  border: 1px solid #333;
}
```

###TODO:
- Automated palette generator
- Fill out documentation
