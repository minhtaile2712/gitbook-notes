# CSS

## Class

:active :checked :first :first-child :hover :not() :nth-child() :nth-of-type()

## Image

object-fit fill (default of jpg): stretch to fill contain (default of svg): sized to fit cover: sized to fill, clip none scale-down: not scale-up, sized to fit



display: flex

flex-direction: row (default) | row-reverse | column | column-reverse

flex-wrap: nowrap (default) | wrap

flex-flow: (flex-direction and flex-wrap)

justify-content: flex-start (default) | flex-end | center | space-around | space-between

align-items: flex-start | flex-end | center | stretch (default) | baseline

align-content: flex-start | flex-end | center | stretch (default) | space-around | space-between

```
	One-line Layout (default)
		align-content is ignored

	Multi-line Layout
		flex-wrap: wrap
		align-items: stretch is ignored
		justify-content: stretch is ignored
		align-content can be used (default: stretch)
		

	Perfect Centering
		justify-content: center
		align-items: center
```

flex-grow: 0 (default) flex-shrink: 1 (default) flex: \[grow shink basis]: 0 1 auto (default)

```
	Consistent Sized Item:
		flex: 0 0 200px
```

window.outerWidth: is the width of the outside of the browser window including all the window chrome.

window.innerWidth: is the width, in CSS pixels, of the browser window viewport including, if rendered, the vertical scrollbar

document.documentElement.clientWidth: is the inner width of a document in CSS pixels, including padding (but not borders, margins, or vertical scrollbars, if present). This is the viewport width. (width of html tag)

document.body.clientWidth (width of body tag) = html when margin body = 0

function getWidth() { console.log("window.innerWidth", window.innerWidth); console.log("window.outerWidth", window.outerWidth); console.log( "documentElement.clientWidth", document.documentElement.clientWidth ); console.log("document.body.clientWidth", document.body.clientWidth); }

## Fonts

### Format a text

text-align font-weight text-decoration line-height letter-spacing

```
@font-face {
    font-family: name;
    src: local(Example Font),
         url() format("truetype|opentype|woff|woff2|");
    font-style: normal|italic|oblique;
    font-weight: normal|bold|100|200|...|900;

}

font: font-size font-family otheroptional;
```
