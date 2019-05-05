# SASS - (Syntactically Awesome StyleSheets)
### `By @ramon_vilafer`
---
SASS (or Syntactically Awesome StyleSheets) is a variation of CSS, which allows you to write modern and expresive sytlesheets very quickly. 

`.scss` files are _transpiled_ to `.css`, usually using node.js, but also with _really cool_ extensions in your editor (like _live scss compile_) for VSCode.

## Standard CSS
It may seem obvious, but you can use standard CSS.
```scss
header{
    background: lightblue;
    display: flex;
    justify-content: center;
}

div section {
    border-radius: 3em;
}
```

## Constants
If you find yourself repeating a value all along your code, then SASS constants may seem like a great idea!

Some ideas are:
- Paddings, margins...
- Colors
- Sizes

```scss
/* Defining constants */
$primaryColor = rgb(56, 146, 142); 
$hoverColor = green;

header{
    /* Don't forget the dollar sign when using them! */
    background: $primaryColor; 
    display: flex;
    justify-content: center;
}

header p {
    background: $primaryColor;
    border: 1pt black dotted;
}

header button {
    display: flex;
    justify-content: center;
    background: $primaryColor;
}

header button:hover {
    background: $hoverColor;
}
```

## Nested selectors
Nothing special here, these are mainly used to improve code readiblity.

Note the ampersand (&) when using a CSS pseudoclass
```scss
$primaryColor = rgb(56, 146, 142);

header{
    background: $primaryColor;
    display: flex;
    justify-content: center; 

    p {
        background: $primaryColor;
        border: 1pt black dotted;
    }

    button {
        display: flex;
        justify-content: center;
        
        /* 
         * This rules apply to all hovered 
         * buttons in the header section */
        &:hover{
            background: $hoverColor;
        }
    }
}
```

## Imports and multiple files
Let's suppose we have the following `main.scss`:

#### main.scss
```scss
$primaryColor = rgb(56, 146, 142);
$hoverColor   = rgb(12,35,85);
$dangerColor  = rgb(0,25,30);

header{
    background: $primaryColor;
    display: flex;
    justify-content: center; 
    p {
        background: $primaryColor;
        border: 1pt black dotted;
    }
    button {
        display: flex;
        justify-content: center;
        &:hover {
            background: $hoverColor;
        }
        &:focus {
            background: $dangerColor;
        }
    }
}

footer {
    background : $dangerColor;
    display: flex;
    justify-content: center;
    float: right;
    padding: 25px;

    .selectedItem {
        background: $hoverColor;
    }
}
```

Then we could split it like this:

#### main.scss
```scss
/* This is the file that you would transpile to CSS and add to your HTML source*/
@import "./header";
@import "./footer";
```

#### _variables.scss
```scss
$primaryColor = rgb(56, 146, 142);
$hoverColor   = rgb(12,35,85);
$dangerColor  = rgb(0,25,30);
```

#### _header.scss
```scss
@import "./variables";

header{
    background: $primaryColor;
    display: flex;
    justify-content: center; 
    p {
        background: $primaryColor;
        border: 1pt black dotted;
    }
    button {
        display: flex;
        justify-content: center;
        &:hover {
            background: $hoverColor;
        }
        &:focus {
            background: $dangerColor;
        }
    }
}
```

#### _footer.scss
```scss
@import "./variables";

footer {
    background : $dangerColor;
    display: flex;
    justify-content: center;
    float: right;
    padding: 25px;

    .selectedItem {
        background: $hoverColor;
    }
}
```
Note the underscore (`_`) previous to all filenames, except `main.scss` (which is the one importing the rest)

## Mixins
Mixins are _like little functions_, that group a bunch of different rules. They are __very__ useful to reduce code size and increase readability.

Following the previous example, we could rewrite it like this using mixins :

#### main.scss
```scss
$primaryColor = rgb(56, 146, 142);
$hoverColor   = rgb(12,35,85);
$dangerColor  = rgb(0,25,30);

/* Declaring a mixin */
@mixin flexCenter {
    display: flex;
    justify-content: center;
}

header{
    background: $primaryColor;

    /*
     * To use it, it's mandatory to use the
     * @include keyword followed by the mixin name
     */
    @include flexCenter();

    p {
        background: $primaryColor;
        border: 1pt black dotted;
    }

    button {
        @include flexCenter();

        &:hover {
            background: $hoverColor;
        }
        &:focus {
            background: $dangerColor;
        }
    }
}

footer {
    background : $dangerColor;
    @include flexCenter();
    float: right;
    padding: 25px;

    .selectedItem {
        background: $hoverColor;
    }
}
```

Mixins can also receive parameters (don't forget the dollar sign!):

```scss
$primaryColor = rgb(56, 146, 142);
$hoverColor   = rgb(12,35,85);
$dangerColor  = rgb(0,25,30);

@mixin verticalPadding($top, $bottom){
    padding-top: $top;
    padding-bottom: $bottom;
}

header{

    p {
        background: $primaryColor;
        border: 1pt black dotted;
    }

    button {

        /*
         * If you pass sizes to your mixins, then
         * don't forget the units, otherwise they won't work!
         */
        @include verticalPadding(10px, 25px);

        &:hover {
            background: $hoverColor;
        }
        &:focus {
            background: $dangerColor;
        }
    }
}
```

## Inheritance

The following snippet has some repeated code. Wouldn't it be very nice if we could just inherit the style from other element?

Worry no more, because you can!
```scss
$primaryColor = rgb(56, 146, 142);
$hoverColor   = rgb(12,35,85);
$dangerColor  = rgb(0,25,30);

header{
    span {
        background: $primaryColor;
        border: 1pt black dotted;
    }
}

.colorBox {
    @extend header;
}
```

## Arithmetics!
SASS allows you to use the basic arithmetic operators:
`+ - / *`, here is an example:
```scss
header{
    span {
        height: 100% - (0.25*1vw);
        width: 35px * 3;
    }
}
```