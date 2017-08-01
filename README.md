# replot-core: Common features across replot (WIP)
Standard implementations of visualization components, such as axes, tooltips, etc.
The core library currently contains a Tooltip, a Resize, and a LoadingIcon
component. To use any of these in your project, simply npm install `replot-core`
and then import the desired utilities in object-literal form.

Example:
```javascript
import {Tooltip, Resize} from "replot-core"
```
will allow you to use the Tooltip and Resize components.

# API
Instructions for utilizing the different components of replot-core

## Tooltip
Allows for the display of detailed information on a mouse hover

### Basic Usage
To implement the tooltip in a Replot component, add the Tooltip tag to the return
section in the render function of your component, specifying certain props at minimum:
a mouse X position, a mouse Y position, an active boolean, and contents. These
should all be state attributes of the replot component, updated through some type
of mouse movement.

```javascript
<Tooltip
  x={this.state.mouseX} y={this.state.mouseY}
  active={this.state.mouseOver} contents={this.state.contents}
/>
```

### Tooltip coloring
There are a few different ways to specify Tooltip coloring. The tooltip comes with
2 default color schemes, light and dark, and either can be chosen by specifying
a "light" or "dark" `colorScheme` prop. Alternatively, the user can completely
customize the Tooltip by passing in hex colors for the `backgroundColor`,
`borderColor`, and `fontColor`.

The Tooltip defaults to the dark color scheme.

## Resize
The Resize component is a wrapper component that will intelligently handle component sizing.

### Basic Usage
To implement the Resize component, you should import and wrap your entire replot
component in the Resize component. The Resize component should receive a
`width` prop which was the user intended width of the original component class.
Then, to maintain the props passed in by the user, also pass `...this.props` directly
to the original component. Refer to this example in the treemap component for usage. Here,
`<TreeMapManager />` was the original component class, and it is now delivered as a
`<TreeMapManagerResponsive/>`

```javascript
class TreeMapManagerResponsive extends React.Component {

  render() {

    return (
      <Resize width={this.props.width}>
        <TreeMapManager {...props} />
      </Resize>
    )
  }
}
```

## LoadingIcon
The LoadingIcon is a basic component that displays an aesthetic placeholder
visual.

### Basic Usage
To use the LoadingIcon, simply import the `{LoadingIcon}` component, and insert
a `<LoadingIcon/>` into your JSX. By default, the LoadingIcon is 100 pixels wide,
and colored black. To change these, the user can pass in a `width` prop to
specify width, and/or a `color` prop with a string-value color to specify the
fill of the dots.

## Legend

### Basic Usage
To use the Legend, import the `{Legend}` component, and insert a
`<Legend />` into your JSX. **Note:** The Legend must be used within an `<svg>`
parent component, since the Legend itself is comprised of SVG elements.

The only required prop is `values`, which should be an object where each key will
be a title in the legend, and each value will be the color associated with the
title.

By default, the Legend lays out titles flat, with a max width of 500 pixels. In
this default flat mode, the user can specify a `width` prop if they wish the Legend
to be thinner or wider.

Alternatively, if the user passes in a `mode` prop with a value of `"stack"`, the
Legend will place titles one on top of the other, using the minimum height and
width necessary to contain all the titles. In `"stack"` mode, the user can
specify a `height` prop, which will keep one title per line, but space out the
titles up to the specified height.

#### Further Customization

The user can pass in additional props to further customize the Legend:
* `showBorder` defaults to `true`, but will not draw a border around the Legend if a
value of `false` is passed
* `borderColor` defaults to `"#000000"`, and will change the color of the border.
* `backgroundColor` defaults to `none`, and will change the background color
or the Legend.

## Axis
Users can import an entire set of axes, or import just the YAxis or XAxis. The
full options include:
- {Axis}
- {YAxis}
- {XAxisDiscrete} :intended for situations where the x axis requires labels
- {XAxisContinuous} :intended for situations where the x axis is a range of numbers

The entirety of customization options/props for the Axis follows:
* `x` and `y`
  * Determines where the upper left corner of the axis will lie
  * Defaults to `0` and `0`
  * Accepts any number
* `width` and `height`
  * Determines the width and height of the axes created, the axes will never
  exceed the given properties, so titles and labels will be drawn within and contents
  may need to be pushed in to compensate
  * Defaults to `400` and `400`
  * Accepts any number
* `xAxisMode`
  * To be used when implementing a full `Axis` component, determines whether the
  x axis will utilize word labels or number ticks
  * Defaults to `"discrete"`
  * Accepts `"discrete"` and `"continuous"`
* `xScale` and `yScale`
  * Determines how the numbers on axes will be distributed
  * Defaults to `"lin"`
  * Accepts `"lin"` and `"log"`
  * **Note:** `xScale` will only function with a `"continuous"` `xAxisMode`
* `minY`, `maxY`, `minX`, `maxX`
  * Used to calculate the spread for axes
  * Any `min` defaults to 0 and any `max` defaults to 100
  * Accepts any number
* `showXAxisLine`, `showXLabels`, `showYAxisLine`, `showYLabels`, and `showGrid`
  * Determines whether or not to show the corresponding element
  * Each default to `true`
  * Accepts `true` or `false`
* `axisStyle`
  * An object that is used to style the axes. Possible key-value pairs include:
    * axisColor
      * modifies the color of the axis line
      * defaults to `#000000`
      * accepts any color string
    * labelColor
      * modifies the color of both axis labels
      * defaults to `#000000`
      * accepts any color string
    * titleColor
      * modifies the color of all graph titles
      * defaults to `#000000`
      * accepts any color string
    * labelColor
      * modifies the color of axis gridlines
      * defaults to `#DDDDDD`
      * accepts any color string
    * lineWidth
      * modifies the thickness of axis lines
      * defaults to `2`
      * accepts any number
    * lineOpacity
      * modifies the opacity of axis lines
      * defaults to `1`
      * accepts any number
  * Example `axisStyle` prop: `{
      axisColor: "#f17e33",
      labelColor: "blue",
      titleColor: "#000000",
      gridColor: "#DDDDDD",
      lineWidth: 5,
      lineOpacity: .5
    }`

### Axis Legends
The Axis component also supports the display of a legend, in multiple modes.
When used with the Axis, the legend will activate if the Axis component is passed
a `legendValues` prop. The `legendValues` prop should be an object where each key will
be a title in the legend, and each value will be the color associated with the
title.

By default, the legend functions in `"flat"` mode, where it lies at the bottom of
the graph underneath all of the labels and titles. The user can pass in a
`legendMode` prop to the Axis component with values of `"stack-inside"` or `"stack-outside"`.
In these modes, the values in the legend lie one on top of the other, and are
positioned in the top-right corner of the graph. If `"stack-inside"` is specified,
the legend will lie within the contents of the graph. If `"stack-outside"` is
specified, the legend will lie outside and push the graph contents to the left.

The Legend can be disabled entirely if a `showLegend` prop is passed in to the
Axis component with a value of `false`.

The Legend can be customized with a single `legendStyle` object prop passed to the Axis
component, with any of the following keys:
* `backgroundColor`
  * Determines the background color of the legend
  * Defaults to `"none"`
  * Accepts any color string
* `fontColor`
  * Determines the color of the text within the legend
  * Defaults to `"#000000"`
  * Accetps any color string
* `showBorder`
  * Determines if a border will be displayed around the legend
  * Defaults to `true`
  * Accepts a boolean value
* `borderColor`
  * Determines the color of the border
  * Defaults to `"#000000"`
  * Accepts any color string

### Buffers to keep in mind when using the axes
If you choose to implement the Axis component, you must keep in mind that the
core elements of your component (such as the bars of a bargraph or the lines
of a linechart) will not begin at (0,0) on a coordinate system, since the axes
themselves take up some space. To compensate, one can make the core elements a
child SVG placed at a certain (x,y). To determine that position, refer to the
information below:

From the top:
* The graph always has a buffer of 5 pixels to allow for the uppermost y-value to
display
* If a graph title is included, the buffer will increase by 25 pixels

From the left:
* The graph always has a buffer of 50 pixels to allow for y-values to display
* If a yTitle is included, the buffer will increases by 25 pixels

From the bottom:
* The graph always has a buffer of 25 pixels to allow for x-values to display
* If an xTitle is included, the buffer will increases by 25 pixels.
* If a flat legend is included, the buffer will further increase by 60 pixels

From the right:
* There is 0 buffer from the right by default
* If the xAxis is continuous, the buffer increases by 25 pixels to allow the
upper most x-value to display
* If a stack-outside legend is included, the buffer increases by 125 pixels.
