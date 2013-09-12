Slides for intro to d3 tutorial
===============================

To run locally, install [reveal-md] (via npm) and run:

    npm install reveal-md
    reveal-md slides.md --theme solarized

Or you can use `grunt server`.

To merge into `gh-pages` branch:

    git checkout gh-pages
    git merge master
    git checkout master
    git push --all
