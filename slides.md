## Introduction to d3

<br><br>

John Goodall

jgoodall@ornl.gov

http://jgoodall.me/

---

## d3

* Created by Mike Bostock, who has [lots of great examples](http://bost.ocks.org/mike/)
* Inherits lots of lessons from Prefuse (java), Flare (flash), and Protovis (javascript)
* Library for creating web-based visualizations
* Can be used with HTML, **SVG**, or Canvas

---

## Some examples

* [d3 show reel](http://bl.ocks.org/mbostock/1256572)
* [Scatterplot matrix](http://bl.ocks.org/mbostock/4063663)
* [Fisheye](http://bost.ocks.org/mike/fisheye/)

<br><small>[see more examples](https://github.com/mbostock/d3/wiki/Gallery)</small>

---

## Declarative

* d3 is basically a DSL for binding data to the Document Object Model (DOM)
* Apply data-driven transformations to the document
* Tell d3 **what** you want done, not how to do it
* This is not like OpenGL or other graphics packages
* *Functions* are everywhere in d3

---

## Skeleton

What does this do?

```
  
console.log('append some text to the body of the page');

d3.select('body')
  .append('text')
    .text('d3 version ' + d3.version + ' is loaded.');

```
<small>[run](http://bl.ocks.org/jgoodall/6538221)</small>

---

## Selection

* Selectors are patterns to select items from the DOM
* Selections are a subclass of an array
* Actually, selections are *arrays of arrays of elements*: a selection is an array of groups, and each group is an array of elements
* d3 uses [css3 selectors](http://www.w3.org/TR/css3-selectors/)
* Selections are transient
* `d3.select(selector)`: Selects the first element that matches the specified selector string, returning a single-element selection
* `d3.selectAll(selector)`: Selects all elements that match the specified selector

<br><small>[read more about selections](http://bost.ocks.org/mike/selection/)</small>

---

## Selector examples

What do these select?
```

d3.select('body')

d3.select('#anId')

d3.select(this)

d3.selectAll('.circles')

d3.selectAll('p')

d3.selectAll('div.content')
```

---

## Selector examples

```

d3.select('body')  // selects the entire, single body of the document

d3.select(this)  // selects the referenced node in an event listener

d3.select('#anId')  // selects the element with the id of 'anId'

d3.selectAll('.circles')  // selects all elements with class 'circle'

d3.selectAll('p')  // selects all paragraph elements on a page

d3.selectAll('div.content')  // selects all divs with class 'content'
```

---

## Dynamic properties

```

d3.selectAll("p")
    .data([4, 6, 8, 10, 14, 16, 24, 42, 88])
    .enter()
  .append("p")
    .style("font-size", function(d) { return d + "px"; })
    .text(function(d, i) { 
      return 'Paragraph ' + i + ' is sized at ' + d + ' pixels.';
    });

```

<small>[run](http://bl.ocks.org/jgoodall/6538427)</small>

---

## Functions are everywhere

* Almost all of the methods in d3 accept an anonymous function. This is a very common pattern: 

```

someOperator(..., function(d, i) {return somethingToDoWithDatum;})
```

* `d` refers to the current selected datum. `i` refers to the index of the datum
* This anonymous function is often called the *accessor* function, because it is often used to tell the operator how to access the data and what to do with it

---

## Example accessor functions

What do these do?

```

  something.attr('width', function(d, i) {
    return w * i;
  });

  something.style('color', function(d, i) {
    return i % 2 ? "#fff" : "#bbb";
  });

  something.text(function(d) {
    return 'Name: ' + d.name + ', phone: ' + d.phone;
  });

```

---

## Data-join

* Data is bound to the DOM - this is called a *data-join*
* `data([someData])` returns three virtual selections: `enter`, `update` and `exit`
* *Enter*: If there is no existing data, all data ends up as placeholder nodes in `enter()`
* *Update*: The update selection contains existing elements, bound to data
* *Exit*: Anything left over are in the exit selection for removal

---

## General update pattern

```
var circle = svg.selectAll("circle")
    .data(data);

circle.enter().append("circle")
    .attr("r", 2.5);

circle
    .attr("cx", function(d) { return d.x; })
    .attr("cy", function(d) { return d.y; });

circle.exit().remove();
```

---

## Enter, Update, and Exit

* If the new dataset is smaller than the old one, the surplus elements end up in the exit selection and get removed
* If the new dataset is larger, the surplus data ends up in the enter selection and new nodes are added
* If the new dataset is exactly the same size, then all the elements are simply updated with new positions, and no elements are added or removed

---

## Now with transitions

```
var circle = svg.selectAll("circle")
    .data(data);

circle.enter().append("circle")
    .attr("r", 0)
  .transition()
    .attr("r", 2.5);

circle
    .attr("cx", function(d) { return d.x; })
    .attr("cy", function(d) { return d.y; });

circle.exit().transition()
    .attr("r", 0)
    .remove();
```

<br><small>[read more about data-joins](http://bost.ocks.org/mike/join/)</small>

---

## Object constancy

* By default, data-joins use the index value for enter, update, exit ([example](http://bl.ocks.org/mbostock/3808218))
* Use a *key function* with the `data()` method ([example](http://bl.ocks.org/mbostock/3808221))
* Use transitions with the general update pattern to make everything smoother ([example](http://bl.ocks.org/mbostock/3808234))

<br><small>[read more about object constancy](http://bost.ocks.org/mike/constancy/)</small>

---

## Working through an example

1. [a basic scatterplot](http://bl.ocks.org/jgoodall/6539774)
2. [scales and axes](http://bl.ocks.org/jgoodall/6539788)
3. [transitions and animation](http://bl.ocks.org/jgoodall/6539804)

---

## Using d3: the quick and dirty

* Create a [gist](https://gist.github.com/) and view on [bl.ocks.org](http://bl.ocks.org/)
* Use [tributary.io](http://tributary.io/)

---

## Using d3: mildly better

* Link to d3js in your `script` tag
```
<script src="http://d3js.org/d3.v3.min.js">
```
* Use a CDN to load, see [cdnjs.com](http://cdnjs.com/)
* Get the source and copy to your project:
```
git clone https://github.com/mbostock/d3.git
```

---

## Fire up a web server

* Using Python

```
python -m SimpleHTTPServer 8888 &
```

* Or, Python 3+

```
python -m http.server 8888 &
```

* Using [nodejs](http://nodejs.org/), using [http-server](https://github.com/nodeapps/http-server)

```
npm install http-server -g
/usr/local/share/npm/bin/http-server -p 8000
# or create an alias
alias ws='/usr/local/share/npm/bin/http-server -p 8000'
ws
```

---

## Using d3: for reals

* Use [bower](http://bower.io/)
```
bower install d3 --save
```
* To see if a newer version exists, run
```
bower list
```
* Even better, use a module loader like [require.js](http://www.requirejs.org/):
```
  paths: {
    'd3': '../bower_components/d3/d3.min'
  }
  shim: {
    d3: {
      exports: 'd3'
    }
```

---

## Libraries on top of d3

* [nvd3](http://nvd3.org/) *Reusable d3 components* - Seems pretty nice. Weird licensing issues
* [dc](http://nickqizhu.github.io/dc.js/) *D3 + Crossfilter* – Seems like it does a lot
* [d3.chart](http://misoproject.com/d3-chart/) *Framework for reusable d3 charts* – Nicely designed abstraction

---

## Crossfilter

* [crossfilter](https://github.com/square/crossfilter) is library that can be used for fast *data filtering and grouping*
* Very useful for linked views
* [Crossfilter demo](http://square.github.io/crossfilter/)

---

## Working with utility libs

* [Jquery](http://jquery.com/) *DOM manipulation, Ajax, etc.* - Used everywhere
* [Lodash](http://lodash.com/) *Data manipulation* - Nicer than using d3 sometimes for working with arrays, collections and objects (cf. [underscore](http://documentcloud.github.io/underscore/))
* That said, d3 does almost all of this...

---

## Working with frameworks

* [Backbone](http://backbonejs.org/) *MVC-ish* - Low barrier to getting started, but will probably be looking elsewhere...
* [Angular](http://angularjs.org/) *HTML extension, data binding* - It is probably awesome. Or just weird. See [Brian Ford's post](http://briantford.com/blog/angular-d3.html) for working with Angular and d3.
* [Flight](http://flightjs.github.io/) *event-driven* - Tries to make you write more modular and reusable code.

---

## Articles

* [Thinking with Joins](http://bost.ocks.org/mike/join/)
* [How Selections Work](http://bost.ocks.org/mike/selection/)
* [Object Constancy](http://bost.ocks.org/mike/constancy/)
* [Let's Make a Map](http://bost.ocks.org/mike/map/)
* [Nested Selections](http://bost.ocks.org/mike/nest/)

---

## Resources

* [Tutorials](https://github.com/mbostock/d3/wiki/Tutorials)
* [Wiki](https://github.com/mbostock/d3/wiki)
* [API Reference](https://github.com/mbostock/d3/wiki/API-Reference)
* [Examples](https://github.com/mbostock/d3/wiki/Gallery)
* [Google Group](http://groups.google.com/group/d3-js)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/d3.js)

---

## Introduction to d3

#### Thank you

<br><br>

John Goodall

jgoodall@ornl.gov

http://jgoodall.me/

