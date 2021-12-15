diffpy.github.io
================

Sphinx sources for the [diffpy.org][site] web page.

### Quick Jump:

- [Structure](#structure)
- [Where to Make Changes](#where-to-make-changes)
  - [Adding Citations](#adding-citations)
    - [Adding Publications that Describe a DiffPy Project (the "Reference" Section)](#reference-section-2)
    - [Adding Other Publications (the "Publication Using DiffPy-CMI" Section)](#reference-section-2)
- [Publishing Changes](#submitting-changes)
- [Updating Project Pages](#updating-project-pages)


# GitHub Structure

This repository contains 3 branches:

1. [`source`][source] sphinx sources for the web page
2. [`master`][master] sources compiled to HTML format that are published via GitHub pages
3. [`archive`][archive] converted subversion repository for the old diffpy.org sources

Please, see the [Wiki](https://github.com/diffpy/diffpy.github.io/wiki) for more
information and tips about this project.


# Where to Make Changes

## Adding Citations

<a name="reference-section-1"></a>
### Adding Publications that Describe a DiffPy Project (the "Reference" Section)

When adding a new publication to the list of [references used within the website](https://www.diffpy.org/publications.html), add the citation text of the publication to the documentation *only once* as a named snippet in [abbreviations.txt](https://github.com/diffpy/diffpy.github.io/blob/source/abbreviations.txt#L286). For example:
```
.. |citeJuhasJac18| replace::
   P. Juhás, J. N. Louwen, L. van Eijck, E. T. C. Vogt, S. J. L. Billinge,
   `PDFgetN3: atomic pair distribution functions from
   neutron powder diffraction data using ad hoc corrections
   <https://doi.org/10.1107/S1600576718010002>`__,
   *J. Appl. Crystallogr.*, **51**, 1492--1497 (2018)
   |downloadJuhasJac18|
```
Here, `|citeJuhasJac18|` is the name that can be used elsewhere in the documentation and the Sphinx documentation generator will replace all instances of this tag with the indented text following the `replace::` directive.

**Important:** After defining the publication's tag as described above, make sure to add the publication to the list of publications maintained in [publications.rst](https://github.com/diffpy/diffpy.github.io/blob/source/publications.rst). Make sure that you add the reference to the proper section and do so in descending reverse chronological order (i.e., the newest citations should appear at the top of their respective sections).

<a name="reference-section-note">*Note:*</a> In this example, the citation is for a publication which describes a product of the DiffPy-CMI project (namely, PDFgetN3). For publications which describe a component of DiffPy-CMI, we provide a link to download the publication directly from the [diffpy.org][site] website. Here, the link is provided via the `|downloadJuhasJac18|` tag which is the identifier for another snippet within [abbreviations.txt](https://github.com/diffpy/diffpy.github.io/blob/source/abbreviations.txt#L294) following the definition of `|citeJuhasJac18|`, seen here as:
```
.. |downloadJuhasJac18| image:: /images/pdficon_small.png
   :target: /doc/pdfgetx/Juhas-jac-2018.pdf
```
Furthermore, note that since the initial use of `|downloadJuhasJac18|` occurs within the indented text of the definition of `|citeJuhasJac18|`, the link to `|downloadJuhasJac18|` will appear everywhere `|citeJuhasJac18|` does.

Lastly, make sure that the linked publication has been included within this project's files, placed in an appropriate directory (typically, the parent directory of the project that the paper relates to). In the above example, the publication covers the PDFgetN3 feature of the `pdfgetx` package, thus it is placed within `pdfgetx`'s documentation directory and referenced accordingly with the line
```
   :target: /doc/pdfgetx/Juhas-jac-2018.pdf
```


<a name="reference-section-2"></a>
### Adding Other Publications (the "Publication Using DiffPy-CMI" Section)

Adding references to publications that do not describe the release/use of a product within the DiffPy-CMI project (e.g., papers which use some component of DiffPy-CMI), we simply provide the usual citation text (with appropriate DOI link). To add a citation of this type, refer to the information in [Reference Section](#reference-section-1), but disregard everything starting at, and following, the [Note](#reference-section-note).


<a name="new-version"></a>
# Publishing New Version of Existing Project

For releasing an updated version of a project, first follow the release procedure outlined in the [group wiki](https://gitlab.thebillingegroup.com/resources/group-wiki/-/wikis/Finalizing-a-Project's-(Re)-Release) for packaging and deploying your project.

In what follows, [pdfgetx v2.1.1](https://github.com/diffpy/diffpy.pdfgetx/releases/tag/v2.1.1) was chosen to show example commands for the steps outlined.

After following the steps necessary for releasing your project, grab the set of documentation that will be provided with the deliverable upon user's request (typically, this would be something like the files hosted by GitHub on the GitHub releases page). Create a new directory for the updated version's documentation to live in
```
$ mkdir diffpy.github.io/doc/pdfgetx/2.1.1
```
Copy the updated documentation to this directory
```
$ tar --directory=static_root/doc/pdfgetx/2.1.1/ \
  --strip-components=4 \
  -vxzf ~/Downloads/diffpy.pdfgetx-2.1.1.tar.gz \
  diffpy.pdfgetx-2.1.1/doc/manual/_build/PDFgetXNS3_manual.pdf

$ tar --directory=static_root/doc/pdfgetx/2.1.1/ \
  --strip-components=5 \
  --exclude=objects.inv \
  --exclude=.buildinfo \
  -vxzf ~/Downloads/diffpy.pdfgetx-2.1.1.tar.gz \
  diffpy.pdfgetx-2.1.1/doc/manual/_build/html/

$ cp ~/Downloads/pdfgetxn3-examples.zip static_root/doc/pdfgetx/2.1.1/
```
Make sure to include all relevant files (e.g., `PDFgetXNS3_manual.pdf` and `pdfgetxn3-examples.zip`), exclude any files we don't want to provide to the user (e.g., `objects.inv` and `.buildinfo` from `diffpy.pdfgetx-2.1.1.tar.gz`)

Finally, edit the [landing page of your project](https://github.com/diffpy/diffpy.github.io/blob/source/products/pdfgetx.rst) (in the [`source` branch][source]) to properly document and provide the updated version of your project. Once the preceding steps are complete, see the instructions of the [Publishing Changes](#submitting-changes) section for publishing these changes.


# Publishing New Project

For adding a new project to the website, see one of the existing projects (e.g., [pdfgetx](https://www.diffpy.org/products/pdfgetx.html)) as reference.

In what follows in this section, you will be working within the [`source`][source] branch, unless otherwise specified.

You will need to create a directory for the project to live in within (e.g., [/static_root/doc/pdfgetx](https://github.com/diffpy/diffpy.github.io/tree/source/static_root/doc/pdfgetx)), then write a landing page for the project (e.g., [/products/pdfgetx.rst](https://github.com/diffpy/diffpy.github.io/blob/source/products/pdfgetx.rst)) which will provide any necessary information or files needed for a user to use the project. Once this is complete, see [Publishing New Version of Existing Project](#new-version) for steps on publishing the project.


# Publishing Changes

In order to update the [diffpy.org][site] page, in your local fork of this repo, edit the files as needed in the [`source`][source] branch, push your changes to your fork's remote (typically called `origin`), then open a pull-request (PR) for the changes to be reviewed and merged. In order to ensure your changes are valid, run the following Makefile commands within the root directory of your local fork of this repo:
```
$ make Makefile linkcheck
```





[site]: <https://www.diffpy.org/> "diffpy.org"
[source]: <https://github.com/diffpy/diffpy.github.io/tree/source> "source"
[master]: <https://github.com/diffpy/diffpy.github.io/tree/master> "master"
[archive]: <https://github.com/diffpy/diffpy.github.io/tree/archive> "archive"
