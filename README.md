statsdc12-presentation
======================

Presentation for the STATS-DC conference in Washington D.C. on 7/11/2012

The source code for the slides is in the statsDCslides.Rmd file (R markdown).

To build the presentation, put this file in the working directory and run:
- `knit('statsDCslides.Rmd')`
- `system("pandoc -s -S -i -t slidy statsDCslides.md -o KnowlesStatsDCslides2012.html --self-contained")`


This calls `pandoc` which must be installed to do the converstion from markdown to HTML5 slides. 

I am looking into the possibility of porting this process to be entirely in R using `slidify`. See future presentations.
