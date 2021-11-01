---
# Please do not edit this file directly; it is auto generated.
# Instead, please edit 03-installing-bioconductor.md in _episodes_rmd/
source: Rmd
title: "Installing Bioconductor"
teaching: XX
exercises: XX
questions:
- "How do I install Bioconductor packages?"
objectives:
- "Install BiocManager."
- "Install Bioconductor packages."
keypoints:
- "BiocManager is used to install Bioconductor packages (but also from CRAN and GitHub), and check for updates."
- "BiocManager safely manages packages from the incremental releases of Bioconductor."
- "The BiocManager package is available from the CRAN repository."
---



# BiocManager

The *[BiocManager](https://bioconductor.org/packages/3.14/BiocManager)* package is the entry point into the Bioconductor package repository.
Technically, this is the only Bioconductor package distributed on the CRAN repository.

It can be installed using the code below.


~~~
install.packages("BiocManager")
~~~
{: .language-r}

The *[BiocManager](https://bioconductor.org/packages/3.14/BiocManager)* package provides functions to safely install Bioconductor packages and check for available updates.

Once the package is installed, the function `BiocManager::install()` can be used to install packages from the Bioconductor repository.
The function is also capable of installing packages from other repositories (e.g., CRAN), if those packages are not found in the Bioconductor repository first.

![The package BiocManager is available from the CRAN repository and used to install packages from the Bioconductor repository.](../fig/bioc-install.svg)

**The package BiocManager is available from the CRAN repository and used to install packages from the Bioconductor repository.**
The function `install.packages()` from the base R package `utils` can be used to install the *[BiocManager](https://bioconductor.org/packages/3.14/BiocManager)* package distributed on the CRAN repository.
In turn, the function `BiocManager::install()` can be used to install packages available on the Bioconductor repository.
Notably, the `BiocManager::install()` function will fall back on the CRAN repository if a package cannot be found in the Bioconductor repository.

> ## Going further
>
> A number of packages that are not part of the base R installation also provide functions to install packages from various repositories.
> For instance:
> - `devtools::install()`
> - `remotes::install_bioc()`
> - `remotes::install_bitbucket()`
> - `remotes::install_cran()`
> - `remotes::install_dev()`
> - `remotes::install_github()`
> - `remotes::install_gitlab()`
> - `remotes::install_git()`
> - `remotes::install_local()`
> - `remotes::install_svn()`
> - `remotes::install_url()`
> - `renv::install()`
>
> Those functions are beyond the scope of this lesson, and should be used with caution and adequate knowledge of their specific behaviors.
> We recommend reading their respective help page to learn more about each of them.
{: .callout}

# Bioconductor releases and current version

Once the *[BiocManager](https://bioconductor.org/packages/3.14/BiocManager)* package is installed, the `BiocManager::version()` function displays the version (i.e., release) of the Bioconductor project that is currently active in the R session.


~~~
BiocManager::version()
~~~
{: .language-r}



~~~
[1] '3.14'
~~~
{: .output}

Using the correct version of R and Bioconductor packages is a key aspect of reproducibility.
The *[BiocManager](https://bioconductor.org/packages/3.14/BiocManager)* packages uses the version of R running in the current session to determine the version of Biocondutor packages that can be installed in the current R library.

The Bioconductor project produces two releases each year, one around April and another one around October.
The April release of Bioconductor coincides with the annual release of R.
The October release of Bioconductor continues to use the same version of R for that annual cycle (i.e., until the next release, in April).

![Timeline of release dates for selected Bioconductor and R versions.](../fig/bioc-release-cycle.svg)

During each 6-month cycle of package development, Bioconductor tests packages for compatibility with the version of R that will be available for the next release cycle.
Then, each time a new Bioconductor release is produced, the version of every package in the Bioconductor repository is incremented, including the package *[BiocVersion](https://bioconductor.org/packages/3.14/BiocVersion)* which determines the version of the Bioconductor project.
This is the case for every package, even those which have not been updated at all since the previous release.
That new version of each package is earmarked for the corresponding version of R;
in other words, that version of the package can only be installed and accessed in an R session that uses the correct version of R.
This version increment is essential to associate a each version of a Bioconductor package with a unique release of the Bioconductor project.

Following the April release, this means that users must install the new version of R to access the newly released versions of Bioconductor packages.

Instead, in October, users can continue to use the same version of R to access the newly released version of Bioconductor packages.
However, to update an R library from the April release to the October release of Bioconductor, users need to call the function `BiocManager::install()` specifying the correct version of Bioconductor as the `version` option, for instance:


~~~
BiocManager::install(version = "3.14")
~~~
{: .language-r}

This needs to be done only once, as the *[BiocVersion](https://bioconductor.org/packages/3.14/BiocVersion)* package will be updated to the corresponding version, indicating the version of Bioconductor in use in this R library.

> ## Going further
>
> The [Discussion][discuss-release-cycle] article of this lesson includes a section discussing the release cycle of the Bioconductor project.
>
{: .callout}

# Check for updates

The `BiocManager::valid()` function inspects the version of packages currently installed in the user library, and checks whether a new version is available for any of them on the Bioconductor repository.

Conveniently, if any package can be updated, the function generates and displays the command needed to update those packages.
Users simply need to copy-paste and run that command in their R console.

If everything is up-to-date, the function will simply print `TRUE`.


~~~
BiocManager::valid()
~~~
{: .language-r}



~~~
[1] TRUE
~~~
{: .output}

> ## Example of out-of-date package library
>
> In this example, the `BiocManager::valid()` function did not return `TRUE`.
> Instead, it includes information about the active user session, and displays the exact call to `BiocManager::install()` that the user should run to replace all the outdated packages detected in the user library with the latest version available in CRAN or Bioconductor.
>
> ```
> > BiocManager::valid()
> 
> * sessionInfo()
> 
> R version 4.1.0 (2021-05-18)
> Platform: x86_64-apple-darwin17.0 (64-bit)
> Running under: macOS Big Sur 11.6
> 
> Matrix products: default
> LAPACK: /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRlapack.dylib
> 
> locale:
> [1] en_GB.UTF-8/en_GB.UTF-8/en_GB.UTF-8/C/en_GB.UTF-8/en_GB.UTF-8
> 
> attached base packages:
> [1] stats     graphics  grDevices datasets  utils     methods   base     
> 
> loaded via a namespace (and not attached):
> [1] BiocManager_1.30.16 compiler_4.1.0      tools_4.1.0         renv_0.14.0        
> 
> Bioconductor version '3.13'
> 
>   * 18 packages out-of-date
>   * 0 packages too new
> 
> create a valid installation with
> 
>   BiocManager::install(c(
>     "cpp11", "data.table", "digest", "hms", "knitr", "lifecycle", "matrixStats", "mime", "pillar", "RCurl",
>     "readr", "remotes", "S4Vectors", "shiny", "shinyWidgets", "tidyr", "tinytex", "XML"
>   ), update = TRUE, ask = FALSE)
> 
> more details: BiocManager::valid()$too_new, BiocManager::valid()$out_of_date
> 
> Warning message:
> 18 packages out-of-date; 0 packages too new 
> ```
{: .callout}

# Installing packages

The `BiocManager::install()` function is used to install or update packages.
The function first searches for the requested package(s) on the Bioconductor repository, but automatically falls back on the CRAN repository and also supports installation directly from online repositories (e.g., GitHub).

For instance, we can install the *[BiocPkgTools](https://bioconductor.org/packages/3.14/BiocPkgTools)* package:


~~~
BiocManager::install("BiocPkgTools")
~~~
{: .language-r}

Multiple package names can be given at once, as a character vector of package names, for instance:


~~~
BiocManager::install(c("BiocPkgTools", "biocViews"))
~~~
{: .language-r}



~~~
'getOption("repos")' replaces Bioconductor standard repositories, see
'?repositories' for details

replacement repositories:
    CRAN: https://cloud.r-project.org
~~~
{: .output}



~~~
Bioconductor version 3.14 (BiocManager 1.30.16), R 4.1.2 (2021-11-01)
~~~
{: .output}



~~~
Warning: package(s) not installed when version(s) same as current; use `force = TRUE` to
  re-install: 'BiocPkgTools' 'biocViews'
~~~
{: .warning}

# Explore the package universe

Attaching a Bioconductor package to the active user session is done using the `library()` function, like any other R package.


~~~
library(BiocPkgTools)
~~~
{: .language-r}

> ## Note
>
> Attaching a package using the `library()` function makes the user-facing package functions accessible directly by their short name.
> It is possible to directly access the functions of any package in the user library without attaching the package to the active session, using the explicit full syntax `package::function()`.
> For instance, the function `BiocPkgTools::biocExplore()` can be called without attaching the `BiocPkgTools` to the session,
> while it can be called simply as `biocExplore()` once the package is attached.
{: .callout}

> ## Contribute!
>
> - Demonstrate relevant functions of the *[BiocPkgTools](https://bioconductor.org/packages/3.14/BiocPkgTools)* package.
>
{: .callout}

# Finding a suitable package

On the Bioconductor website, the [biocViews][biocviews-webpage] use a predefined - yet, evolving - controlled vocabulary to classify all the packages in the Bioconductor project.

At the top level, labels distinguish four major categories of packages by their nature:

- _Software_ packages, that primarily provide classes and methods to process data and perform statistical analyses and implement analytical workflows.
- _Annotation_ packages, that often provide access to databases of biological information, from biological sequences to the location of genomic features, as well as mapping between identifiers from different databases.
- _Experiment_ packages, that provide standard data sets often used in the vignette of _Software_ packages, to demonstrate package functionality.
- _Workflow_ packages, that primarily provide vignettes demonstrating how to combine multiple _Software_, _Annotation_, and _Experiment_ packages into best practices integrated workflows.

Within each category, [biocViews][biocviews-webpage] create sub-categories that facilitate efficient thematic browsing and filtering by prospective users, navigating the hierarchy of labels to iteratively refine the list of package until they identify candidate packages that provide the functionality they are searching for.

[git-website]: https://git-scm.com/
[git-bioconductor]: https://git.bioconductor.org/
[biocviews-webpage]: https://www.bioconductor.org/packages/release/BiocViews.html
[discuss-package-versioning]: ../discuss/index.html#bioconductor-package-versions
[discuss-release-cycle]: ../discuss/index.html#the-bioconductor-release-cycle
[bioc-release-dates]: https://www.bioconductor.org/about/release-announcements/