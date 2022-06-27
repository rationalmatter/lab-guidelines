# Preparing a Lab

The process is meant to be easy: format your tutorial as a (single) Jupyter notebook while following conventions described below.

<details><summary>This is how a lab will look in the app. Click to expand.</summary>
<p align="center">
  <img src="img/iris-lab-in-app-2.png" width="900" title="App screenshot showing Iris lab">
  <img src="img/iris-lab-in-app-3.png" width="900" title="App screenshot showing Iris lab">
  <img src="img/iris-lab-in-app-4.png" width="900" title="App screenshot showing Iris lab">
  <img src="img/iris-lab-in-app-5.png" width="900" title="App screenshot showing Iris lab">
</p>
</details>

## Chapters and pages
Your Jupyter notebook will be automatically sliced into chapters and pages. You can assume that first-level headers (e.g.`# Title`) will become chapters, second-level headers (e.g. `## Title`) will become pages, and all further-level headers will just appear on the page as-is.

## Folder structure
All data should be in _Data_ folder, and all images from Markdown cells should be in _Assets_ folder. As mentioned down below, additional Python packages go to the _Packages_ directory. 

## Environment
The app comes with a specific Python environment, and labs should be prepared using the same versions of interpreter and packages. You can download the [environment.yml](environment.yml) file from this repo to re-create this exact environment on your machine with conda. You can also use additional pure Python packages, which are not in the .yml file — as long as they don't have native extensions (.c files compiled into libraries). Put all additional packages into the _Packages_ folder, and assume they will be available for import anywhere in the notebook. Mind that although `pip` is in the environment file, it will not be available in the app.

> Although you can't use some "big name" packages like TensorFlow or PyTorch (at least for now), current environment already has plenty of packages to work with: SciPy, Scikit-learn, Pandas, LXML, Matplotlib, NumPy, Pillow and others.

## Content guidelines
A few things that will make a lab look and feel its best in the app:
* A lab would usually have narrative structure of a step-by-step tutorial, separated into chapters and pages with continuity in mind.
* Prefer smaller pages and consider breaking down long ones. Keep in mind that _Run Code_ button will execute all code cells on a page at once, so this would work best with pages that fit on a single screen without too much scrolling.
* Use more text and images! Everything that Jupyter supports will be rendered correctly, including interactive Bokeh plots, HTML, LaTeX formulas, etc.

## Bonus Tips
<details><summary>Finally, a few bonus "pro" tips! 😁 These are completely optional to follow, but perhaps they will make your life a bit easier, or let you do something cool. Click to expand.</summary>

### Hidden cells
JupyterLab lets you hide cells by clicking the blue cell selection indicator to the left of the cell. Hidden cells will not be shown on the lab page, but the code in them _will_ get executed with all other code cells on page when user taps _Run Code_ button.

### Different images for light and dark themes
The app supports light and dark interface themes, and you can add separate light and dark variants of an image that you embed into a Markdown cell — with a bit of HTML. For example, under the hood, the app will inject the following CSS into each lab page, whenever (and only when) _dark_ UI theme is active:

```css
.juno_ui_theme_light {
    display: none;
}

.juno_ui_theme_dark {
    display: inline-block;
}
```

Basically, this means that this HTML code in a Markdown cell will display `nn_light.png` image for light UI theme, and `nn_dark.png` when dark mode is enabled:

```html
<img src="Assets/nn_light.png" class="juno_ui_theme_light"><img src="Assets/nn_dark.png" class="juno_ui_theme_dark">
```

### Don't rely too much on narrative context
Don't expect the pages to be opened and executed one after another, the user should be able to jump to an arbitrary page. Don't worry about initialising variables in code cells: the app will correctly set interpreter state for each page, including variables initialised on previous pages, or modules imported earlier. But don't rely on files created on disk in earlier pages, those are not part of the page "context".
  
### Consider using smaller datasets
Try to keep lab's performance footprint at minimum: for example, use smaller datasets. One of our sample labs operates with a smaller MNIST dataset which only has 10,000 examples from the original 70,000 — it makes final labs much smaller, not to mention it takes less time to train a model, while illustrating the point just as well. Remember, all code is executed locally, using device's hardware (so performance could become a constraint for complex computations).

### Clear unused variables
Finally, it's a good practice to delete variables and clear other resources you no longer use in the rest of the lab. Use a hidden code cell at the end of the page for this:

<p align="center">
  <img src="img/hidden-cell-collapsed.png" width="200" title="Collapsed cell"> <img src="img/hidden-cell-expanded.png" width="200" title="Expanded cell">
</p>

Deleting a variable with `del df`, or clearing a Matplotlib figure with `fig.clear()` goes a long way in keeping lab size in check.
</details>
