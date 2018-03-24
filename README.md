# Altair

<a href="https://altair-viz.github.io"><img src="https://altair-viz.github.io/_static/altair-logo-light.png" align="left" hspace="10" vspace="6" alt="Altair logo"></a>

**Altair** is a declarative statistical visualization library for Python. With Altair, you can spend more time understanding your data and its meaning. Altair's
API is simple, friendly and consistent and built on top of the powerful
[Vega-Lite](https://github.com/vega/vega-lite) JSON specification. This elegant
simplicity produces beautiful and effective visualizations with a minimal amount of code. *Altair is developed by [Jake Vanderplas](https://github.com/jakevdp) and [Brian
Granger](https://github.com/ellisonbg) in close collaboration with the [UW
Interactive Data Lab](http://idl.cs.washington.edu/).*

[![build status](http://img.shields.io/travis/altair-viz/altair/master.svg?style=flat)](https://travis-ci.org/altair-viz/altair)

## Altair Documentation

**Note: Altair's documentation is currently in a very incomplete form; we are
in the process of creating more comprehensive documentation. Stay tuned!**

See [Altair's Documentation Site](http://altair-viz.github.io),
as well as Altair's [Tutorial Notebooks](http://github.com/altair-viz/altair_notebooks).

## Example

Here is an example using Altair to quickly visualize and display a dataset with the native Vega-Lite renderer in the JupyterLab:

```python
import altair as alt

# to use with Jupyter notebook (not JupyterLab) run the following
# alt.renderers.enable('notebook')

# load a simple dataset as a pandas DataFrame
from vega_datasets import data
cars = data.cars()

alt.Chart(cars).mark_point().encode(
    x='Horsepower',
    y='Miles_per_Gallon',
    color='Origin',
)
```

![Altair Visualization](images/cars.png?raw=true)

## A Python API for statistical visualizations

Altair provides a Python API for building statistical visualizations in a declarative
manner. By statistical visualization we mean:

* The **data source** is a `DataFrame` that consists of columns of different data types (quantitative, ordinal, nominal and date/time).
* The `DataFrame` is in a [tidy format](http://vita.had.co.nz/papers/tidy-data.pdf)
  where the rows correspond to samples and the columns correspond to the observed variables.
* The data is mapped to the **visual properties** (position, color, size, shape,
  faceting, etc.) using the group-by data transformation.

The Altair API contains no actual visualization rendering code but instead
emits JSON data structures following the
[Vega-Lite](https://github.com/vega/vega-lite) specification. The resulting
Vega-Lite JSON data can be rendered in the following user-interfaces:

* [Jupyter Notebook](https://github.com/jupyter/notebook) (by installing [ipyvega](https://github.com/vega/ipyvega)).
* [JupyterLab](https://github.com/jupyterlab/jupyterlab) (no additional dependencies needed).
* [nteract](https://github.com/nteract/nteract) (no additional dependencies needed).

## Features

* Carefully-designed, declarative Python API based on
  [traitlets](https://github.com/ipython/traitlets).
* Auto-generated internal Python API that guarantees visualizations are type-checked and
  in full conformance with the [Vega-Lite](https://github.com/vega/vega-lite)
  specification.
* Auto-generate Altair Python code from a Vega-Lite JSON spec.
* Display visualizations in the live Jupyter Notebook, JupyterLab, nteract, on GitHub and
  [nbviewer](http://nbviewer.jupyter.org/).
* Export visualizations to PNG/SVG images, stand-alone HTML pages and the
[Online Vega-Lite Editor](https://vega.github.io/editor/#/).
* Serialize visualizations as JSON files.
* Explore Altair with dozens of examples in the [Example Gallery](https://altair-viz.github.io/gallery/index.html)

## Installation

**Note**: Altair version 2.0 requires Python 3.5 or later.
It does not currently work with Python 2.

To use Altair for visualization, you need to install two sets of tools

1. The core Altair Package and its dependencies

2. The renderer for the frontend you wish to use (i.e. `Jupyter Notebook`,
   `JupyterLab`, or `nteract`)

Altair can be installed with either ``pip`` or with ``conda``.
For full installation instructions, please see
https://altair-viz.github.io/getting_started/installation.html

## Example and tutorial notebooks

We maintain a separate Github repository of Jupyter Notebooks that contain an
interactive tutorial and examples:

https://github.com/altair-viz/altair_notebooks

To launch a live notebook server with those notebook using [binder](https://beta.mybinder.org/),
click on the following badge:

[![Binder](https://beta.mybinder.org/badge.svg)](https://beta.mybinder.org/v2/gh/altair-viz/altair_notebooks/master)

## Project philosophy

Many excellent plotting libraries exist in Python, including the main ones:

* [Matplotlib](http://matplotlib.org/)
* [Bokeh](http://bokeh.pydata.org/en/latest/)
* [Seaborn](http://stanford.edu/~mwaskom/software/seaborn/#)
* [Lightning](http://lightning-viz.org/)
* [Plotly](https://plot.ly/)
* [Pandas built-in plotting](http://pandas.pydata.org/pandas-docs/stable/visualization.html)
* [HoloViews](http://holoviews.org)
* [VisPy](http://vispy.org/)
* [pygg](http://www.github.com/sirrice/pygg)

Each library does a particular set of things well.

### User challenges

However, such a proliferation of options creates great difficulty for users
as they have to wade through all of these APIs to find which of them is the
best for the task at hand. None of these libraries are optimized for
high-level statistical visualization, so users have to assemble their own
using a mishmash of APIs. For individuals just learning data science, this
forces them to focus on learning APIs rather than exploring their data.

Another challenge is current plotting APIs require the user to write code,
even for incidental details of a visualization. This results in unfortunate
and unnecessary cognitive burden as the visualization type (histogram,
scatterplot, etc.) can often be inferred using basic information such as the
columns of interest and the data types of those columns.

For example, if you are interested in a visualization of two numerical
columns, a scatterplot is almost certainly a good starting point. If you add
a categorical column to that, you probably want to encode that column using
colors or facets. If inferring the visualization proves difficult at times, a
simple user interface can construct a visualization without any coding.
[Tableau](http://www.tableau.com/) and the [Interactive Data
Lab's](http://idl.cs.washington.edu/)
[Polestar](https://github.com/vega/polestar) and
[Voyager](https://github.com/vega/voyager) are excellent examples of such UIs.

### Design approach and solution

We believe that these challenges can be addressed without the creation of yet
another visualization library that has a programmatic API and built-in
rendering. Altair's approach to building visualizations uses a layered design
that leverages the full capabilities of existing visualization libraries:

1. Create a constrained, simple Python API (Altair) that is purely declarative
2. Use the API (Altair) to emit JSON output that follows the Vega-Lite spec
3. Render that spec using existing visualization libraries

This approach enables users to perform exploratory visualizations with a much
simpler API initially, pick an appropriate renderer for their usage case, and
then leverage the full capabilities of that renderer for more advanced plot
customization.

We realize that a declarative API will necessarily be limited compared to the
full programmatic APIs of Matplotlib, Bokeh, etc. That is a deliberate design
choice we feel is needed to simplify the user experience of exploratory
visualization.

## Development install

Altair requires the following dependencies:

* [pandas](http://pandas.pydata.org/)
* [traitlets](https://github.com/ipython/traitlets)
* [IPython](https://github.com/ipython/ipython)

If you have cloned the repository, run the following command from the root of the repository:

```
pip install -e .
```

If you do not wish to clone the repository, you can install using:

```
pip install git+https://github.com/altair-viz/altair
```

## Testing

To run the test suite you must have [py.test](http://pytest.org/latest/) installed.
To run the tests, use

```
py.test --pyargs altair
```
(you can omit the `--pyargs` flag if you are running the tests from a source checkout).

## Feedback and Contribution

We welcome any input, feedback, bug reports, and contributions via [Altair's
GitHub Repository](http://github.com/altair-viz/altair/). In particular, we
welcome companion efforts from other visualization libraries to render the
Vega-Lite specifications output by Altair. We see this portion of the effort
as much bigger than Altair itself: the Vega and Vega-Lite specifications are
perhaps the best existing candidates for a principled *lingua franca* of data
visualization.

We are also seeking contributions of additional Jupyter notebook-based examples
in our separate GitHub repository: https://github.com/altair-viz/altair_notebooks.

## Whence Altair?

Altair is the [brightest star](https://en.wikipedia.org/wiki/Altair) in the constellation Aquila, and along with Deneb and Vega forms the northern-hemisphere asterism known as the [Summer Triangle](https://en.wikipedia.org/wiki/Summer_Triangle).
