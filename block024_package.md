---
title: "Write your own R package"
author: "Bernhard Konrad"
date: "2014-11-10"
output:
  html_document:
    toc: true
    toc_depth: 2
    pandoc_args: "--preserve-tabs"
---

Write your own R package
================================================================================

This is a step-by-step instruction on how to create your first R package.

+ This will be much easier than you think
+ Bundle helper functions for your other projects
+ Share your code
+ Add (some) documentation and examples
+ Add tests

Getting Started
================================================================================

In this tutorial we will develop a package that provides the function `gameday`. This function takes one argument `teamname`, the name of your favorite [NHL](http://www.nhl.com/) team, and returns `TRUE` if this team has a game today, and `FALSE` otherwise. The function will actually be a one-liner because we can read this information from the web.

### Packages that you will need

Please make sure to run `install.packages('devtools')` and `install.packages('roxygen2')`.

@jennybc: Let's say we assume everyone is running very current R (3.1.0 or higher?) and RStudio. I also recommend that everyone start with `update.packages()`. Specifically, devtools, roxygen2, testthat, and knitr should all be very current, so why not just update everything? 

### Some Background and pointers

You can skip this section on your first read if you want to dive right into the hands-on exercise.

@jennybc: TODO for me. Might link to slides.

### Setting up the folder structure

+ R expects a certain folder structure for your package. Luckily, `RStudio`, using `devtools`, does this work for you.
+ In *RStudio*: *File -> New Project -> New Directory -> R Package*. 
+ Package name `gameday` and please also make sure that **Create a git repository** is checked.

You see that this created a galore of files and folders. Let's see what they are.

@jennybc: No, sadly, RStudio does not (yet) use devtools for this, though apparently it soon will. In the meantime, let's follow the advice given in Hadley's book, which is to use `devtools::create("path/to/package/pkgname")`, followed by double clicking on the associated `pkgname.Rproj` file or RStudio File > New Project > Existing Directory ... pointed at `path/to/package/pkgname` and with **Create a git repository** checked. Here's a side-by-side comparison of freshly created R packages via the two different workflows (I think I forgot to make it a Git repo via RStudio, but that's neither here nor there):

![](img/rstudio-vs-devtools-create-for-new-pkg.png)

### Files that make a packages

* Helper files that we don't have to worry about:
    + `.gitignore` is the usual ignore file for Git. We don't have to change it.
    + `.Rbuildignore` is an ignore file for the R package building process. We can talk about this later.
    + `gameday.Rproj` is the usual file for an RStudio project. We don't have to change it.
    + `NAMESPACE` is a very important file, but we will never edit this by hand. `roxygen2` will maintain this for us.

* `R/` finally, this is where the actual R code will go.
* `DESCRIPTION` holds meta information about your package. We will modify this first.

@jennybc: If you submit to my plea that we init package with `devtools::create()` two  bullet points go away. I have deleted them for you!

### DESCRIPTION FILE

Here is where we add information about the package (gameday) and its authors (us). Some fields are pre-filled, but many more fields can be added as necessary.


    Package: gameday
    Type: Package
    Title: Let R tell you if your NHL team plays today
    Version: 1.0
    Date: 2014-11-11
    Author: Bernhard Konrad, Jennifer Bryan
    Maintainer: Bernhard Konrad <bernhard.konrad@gmail.com>
    Description: Querry live.nhle.com to check if your NHL team is listed on the teams that play today
    License: CC0


Let's briefly discuss some of those fields.
* `Maintainer` is a mandatory, and must provide a contact email
* `License` [*CC0* ](http://creativecommons.org/publicdomain/zero/1.0/) means that we dedicate our package to the public domain and waive all of our rights. Anyone can freely use/adapt/modify/sell this work without our permission. We also don't provide any warranties about liability or correctness. You can check (other creative common licenses)[http://creativecommons.org/choose/].


### The actual R code

The R code that our package provides is in the *R* folder. You see that a file `gameday.R` is already there. We will leave this alone for now. Instead, let's **create a new R script** and save it in the *R* folder with the name `gday.R`.

The content is the following:


    #' Is it Gameday?
    #'
    #' This function returns TRUE if your NHL team plays today
    #' @param teamname Defaults to "canucks"
    #' @param date The (potential game) day you want to check
    #' @return \code{TRUE} if \code{teamname} has an NHL game on \code{date},
    #' \code{FALSE} otherwise
    #' @keywords misc
    #' @note case in \code{teamname} is ignored
    #' @export
    #' @examples
    #' gday()
    #' gday("canadiens")
    #' gday("Bruins", "2014-11-07")

    gday <- function(teamname="canucks") {
      url <- paste0("http://live.nhle.com/GameData/GCScoreboard/", Sys.Date(), ".jsonp")
      grepl(teamname, getURL(url), ignore.case=TRUE)
    }


TODO: explain function gday. Explain roxygen syntax (split the above up in two steps). Add dependency `RCurl` to DESCRIPTION.


### Documentation for the package itself

    #' More documentation
    #' 
    #' and some more stuff
    #' 
    #' and even more details
    #' that spans two lines
    #' 
    #' @docType package
    #' @name gameday
    NULL
    
TODO: explain roxygen syntax and why `NULL`


### Let devtools, roxygen2 do the documenation for you

Run `document()` and *Build & Reload* for testing.


### Publish on GitHub

1. commit files locally
2. Create empty repo on github
3. Add remote: Shell -> Terminal -> git remote add ... -> git push -u origin master
4. install from github

------------------------------------------------------------