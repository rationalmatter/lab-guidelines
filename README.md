# Preparing a Lab

Preparing a lab is fairly easy: you simply put your tutorial together as a single Jupyter notebook while following a couple of formatting conventions, and place image assets and data into predefined locations. Key things to keep in mind:

## Environment
The app comes with a specific Python environment, and labs should be prepared using the same versions of interpreter and packages. The [environment.yml](environment.yml) file in conda format is in this repo. You can use any additional pure Python packages, which are not in the .yml file — as long as they don't have native extensions (i.e. no compiled native code in a form of libraries). Put all additional packages into the _Packages_ folder, and assume they will be available for import anywhere in the notebook.

## Manifests
This is a fancy word for text cells with a simple YAML configuration, which will indicate where each chapter and page begins. Put manifest for every chapter and page in a separate "Raw" cell: 

<p align="center">
  <img src="img/manifest-cell.png" width="450" title="Example of chapter and page manifest cells">
</p>

Mind that `iconIdentifier` here is the name of a [SF Symbols](https://developer.apple.com/sf-symbols/) glyph (you can check out some of them here: https://github.com/cyanzhong/sf-symbols-online) — it is optional, and if you don't provide one, a generic icon will be used. The first cell in notebook must have the lab manifest, the fields should be self-explanatory (please refer to the example lab notebook).

## Folder structure
As mentioned earlier, additional Python packages go to the _Packages_ directory. All data used in notebook should go to the _Data_ folder, and all image assets used in Markdown cells should go to _Assets_ folder. Each page using assets in Markdown cells should specify them in its Manifest:

<p align="center">
  <img src="img/page-assets-in-manifest.png" width="650" title="Example of a page manifest cell using image assets">
</p>

## Content guidelines
A few tips on what kind of content the app was designed for:
1. A lab would usually have narrative structure of a step-by-step tutorial, separated into chapters and pages with continuity in mind.
2. Prefer smaller pages and consider breaking down long ones. Keep in mind that _Run Code_ button will execute all code cells on a page at once, so this would work best with pages that fit on a screen without too much scrolling.
3. Use more text and images! Everything that Jupyter supports will be rendered correctly, including HTML, LaTeX formulas, etc. If you have multiple headings on the page, keep in mind that page title will appear as a level 1 heading at the top of the page, it will be rendered automatically by the app.
4. Don't expect the pages to be opened and executed one after another, the user should be able to jump to an arbitrary page. Don't worry about initialising variables in code cells: the app will correctly set interpreter state for each page, including imported modules and variables initialised on earlier pages. But don't rely on files created on disk in earlier pages, those are not part of the page "context".
5. Consider using smaller datasets. For example, one of our sample labs operates with a smaller MNIST dataset which only has 10,000 examples from the original 70,000. It makes final labs much smaller, not to mention it takes less time to train a model on 10,000 examples, while illustrating the point just as well. Remember, all code is executed locally, using device's hardware.

## Bonus Tips
<details><summary>Finally, a few bonus "pro" tips! 😁 These are completely optional to follow, but perhaps they will make your life a bit easier, or let you do something cool. Click to expand.</summary>
  
### Ignored cells  
Cells that start with "---" are ignored and won't appear in the final lab ("---" is rendered as horizontal line in Jupyter). Could be useful to visually separate pages or chapters while working on the notebook.

### Hidden cells
JupyterLab lets you hide cells by clicking the blue cell selection indicator to the left of the cell. Hidden cells will not be shown on the lab page, but the code in them _will_ get executed with all other code cells on page when user taps _Run Code_ button.

### Separate images for light and dark themes
The app supports light and dark interface themes, and you can add separate light and dark variants of an image that you embed into a Markdown cell — with a bit of HTML. Under the hood, the app will inject the following CSS into each lab page:

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
<img src="Assets/nn_light.png" class="juno_ui_theme_light" style="display: inline-block;"><img src="Assets/nn_dark.png" class="juno_ui_theme_dark" style="display: none;">
```

### Clear unused variables
Finally, it's a good practice to delete variables and clear other resources you no longer use in the rest of the lab. Use a hidden code cell at the end of the page for this:

<p align="center">
  <img src="img/hidden-cell-collapsed.png" width="200" title="Collapsed cell"> <img src="img/hidden-cell-expanded.png" width="200" title="Expanded cell">
</p>

Deleting a variable with `del df`, or clearing a Matplotlib figure with `fig.clear()` goes a long way in keeping lab size in check.
</details>
