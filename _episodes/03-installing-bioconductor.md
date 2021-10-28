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



# Installing BiocManager

The *[BiocManager](https://bioconductor.org/packages/3.14/BiocManager)* package is the entry point into the Bioconductor package repository.
Technically, this is the only Bioconductor package distributed on the CRAN repository.
As such, the package can be installed using the traditional `install.packages()` function.


~~~
install.packages("BiocManager")
~~~
{: .language-r}


# Bioconductor releases and current version

The Bioconductor project produces two releases each year, one around April and another one around October.

The April release of Bioconductor coincides with the annual release of R.
In the months leading up to the April release, packages that will feature in that Bioconductor release are tested using the upcoming version of R (i.e., "R-devel").
To access the newly released version of those packages, users must first install the new version of R.

The October release of Bioconductor continues to use the same version of R for that annual cycle (i.e., until the next release, in April).

Each time a new release is produced, the version of each the packages in the Bioconductor repository is incremented.
This is the case for every package, even those which have not been updated at all since the previous release.
This version increment is essential to associate a unique version of each Bioconductor package with each release of the Bioconductor project.

In particular, the version of the *[BiocVersion](https://bioconductor.org/packages/3.14/BiocVersion)* package is used to represent the version of the Bioconductor project.
In turn, the version of the Bioconductor release currently active in a user session determines the version of Bioconductor packages to be installed by the `BiocManager::install()` function.

Once the *[BiocManager](https://bioconductor.org/packages/3.14/BiocManager)* package is installed, the `BiocManager::version()` function displays the version (i.e., release) of the Bioconductor project that is currently active in the R session.


~~~
BiocManager::version()
~~~
{: .language-r}



~~~
[1] '3.14'
~~~
{: .output}

> ## Going further
>
> The [Discussion][discuss-package-versioning] article of this lesson includes a section discussing the format and intepretation of Bioconductor package versions.
>
{: .callout}

It is possible to specify a version of Bioconductor, which in turn will install the latest version of Bioconductor packages for that particular Bioconductor release.
For instance:

~~~
BiocManager::install(version = "3.14")
~~~
{: .language-r}

Note that `BiocManager::install(version = ...)` can also be used to update a library of Bioconductor packages to a certain version of Bioconductor, either a more recent or an older release.
In that case, it is not necessary to specify any package name.
All the Bioconductor packages installed in the current package library will be replaced by the latest version available for the requested version of Bioconductor.

> ## The Bioconductor release cycle - release and devel branches
>
> ### Release branches
>
> Bioconductor uses the [Git][git-website] version control system to manage its package repository.
> For each new Bioconductor release (i.e., version), a new branch is created in the [Bioconductor Git repository][git-bioconductor]; those are referred to as _release_ branches.
> Release branches allow users to install stable versions of packages that were tested together for a given version of Bioconductor, itself earmarked for a specific version of R.
>
> Work on the _release_ branches is restricted.
> Older _release_ branches are entirely frozen, meaning that no further update is allowed on those branches.
> When users request a package for a given version of Bioconductor, they receive the latest version of the package on the correspoding release branch.
> 
> Only the latest release branch allows updates from package maintainers, but those are restricted to critical bug fixes.
> This means that for each 6-month release cycle, users can expect packages on the latest branch to be reasonably stable.
>
> ### Devel branches
>
> Meanwhile, the main branch of the Git repository (historically called `master`) is referred to as the _devel_ branch.
>
> The _devel_ branch allow developers to continue updating the packages as frequently as they wish, without affecting users or disrupting workflows.
> Typically, packages on the _devel_ branch are mainly used by other developers and the Bioconductor build system, to run tests using the latest code of every package in the Bioconductor repository, and to prepare the next stable release of the project.
> However, users can also access packages on the _devel_ branch using `BiocManager::install(version = ...)` with `version` set to one minor version greater than the latest Bioconductor _release_ version (e.g. if the latest release is `3.13`, then devel is `3.14`).
>
> ### Transition between devel and release - the release process
>
> After a new release branch is created, the version of every single package on the _devel_ branch is incremented, to prepare the version of the package that will feature in the next Bioconductor stable release.
> This includes the *[BiocVersion](https://bioconductor.org/packages/3.14/BiocVersion)* package, which marks the value of the next version of Bioconductor.
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
The function first searches for the requested package(s) on the Bioconductor repository, but  automatically falls back on the CRAN repository and also supports installation directly from online repositories (e.g., GitHub).

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
Bioconductor version 3.14 (BiocManager 1.30.16), R 4.1.1 (2021-08-10)
~~~
{: .output}



~~~
Warning: package(s) not installed when version(s) same as current; use `force = TRUE` to
  re-install: 'BiocPkgTools' 'biocViews'
~~~
{: .warning}



~~~
Old packages: 'lattice', 'mgcv', 'nlme', 'survival'
~~~
{: .output}

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
[bioc-release-dates]: https://www.bioconductor.org/about/release-announcements/