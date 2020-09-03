---
title: "Create a table of content with links for PDF"
excerpt: "Create clickable table of content for PDF without prorietary software"
author_profile: false
categories:
  - PDF
tags:
  - PDFtk
  - bookmark
Layout: posts
toc: true
toc_sticky: true
---

Did you ever think to create a table of content with links to a PDF ebook without a proprietary software? Here is a solution by adding a table of conetent in the form of hyperlink hierarchical bookmarks with [PDFtk](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/ "PDFtk").
{: style="text-align: justify;"}

## Installation of  PDFtk

### Windows

Downlad [HERE](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/pdftk_free-2.02-win-setup.exe "PDFtk Windows installer"), and execute a normal installation.

### Linux
* _KDE Neon 5.19.4 (based on Ubuntu 20.04):_
: PDFtk can be installed with its dependencies in Synaptic.
* _Fedora 32:_
: PDFtk is not contained in any Fedora 32 offical repositories. Fortunately, PDFtk has been built in Fedora 30, and successfully tested in Fedora 32. We can download its rpm packages [HERE](https://sandbox.mc.edu/~bennet/pdftk.html "PDFtk for Fedora 32"). Then, install by the following command-line on your local PC.

`sudo dnf install [downloaded rpm path]`

**Note:** There are two packages to be downloaded. The package `gcc6-libgcj-6.5.0-2.fc30.x86_64.rpm` should be firstly installed since it is the dependency of the package `pdftk-2.02-2.fc30.x86_64.rpm`.
{: .notice--warning}

## Creating table of content with PDFtk

This section consists of three subsections:
1. Export meta data of PDF into a text file
2. Add table of content by editing the text file
3. Import the new meta data (saved in the text file) back to the PDF

### 1. Export meta data of PDF ebook

Using the following command-line to export the meta data of a PDF to a text file:

`pdftk [pdf source] dump_data output [text file]`

For example, we have an ebook named ```JavaCookBook.pdf```, and we want to export its meta data to ```bookmark.txt``` in the same directory. Therefore, we change the current directory to where the PDF file ```JavaCookBook.pdf``` is saved. Then, we type: 

`pdftk ./JavaCookBook.pdf dump_data output ./bookmark.txt`

Here, we find the meta data of the PDF saved in ```bookmark.txt```

### 2. Insert bookmark in meta data of PDF ebook

Open ```bookmark.txt``` in your favorite editor, and you might have the similar content like this:

```
InfoBegin
InfoKey: ModDate
InfoValue: D:20140413195235+08'00'
InfoBegin
InfoKey: CreationDate
InfoValue: D:20140413112352+08'00'
InfoBegin
InfoKey: PXCViewerInfo
InfoValue: PDF-XChange Viewer;2.5.210.0;Feb 25 2013;15:35:42;D:20140413195235+08'00'
InfoBegin
InfoKey: Creator
InfoValue: pdfFactory Pro www.fineprint.cn
InfoBegin
InfoKey: Producer
InfoValue: pdfFactory Pro 3.22 (Windows XP Professional Chinese)
PdfID0: 9ddb8aba3cd22eb6222a807e8238fde8
PdfID1: 9ddb8aba3cd22eb6222a807e8238fde8
NumberOfPages: 987
PageMediaBegin
PageMediaNumber: 1
PageMediaRotation: 0
PageMediaRect: 0 0 595 842
PageMediaDimensions: 595 842
PageMediaBegin
PageMediaNumber: 2
PageMediaRotation: 0
PageMediaRect: 0 0 595 842
PageMediaDimensions: 595 842
PageMediaBegin
PageMediaNumber: 3
PageMediaRotation: 0
PageMediaRect: 0 0 595 842
PageMediaDimensions: 595 842
```

Add the four following tags after the tag ```NumberOfPages:```

```
BookmarkBegin
BookmarkTitle:
BookmarkLevel:
BookmarkPageNumber:
```

The four tags are ALL necessary for each chapter. Repeatedly append these four tags for all the chapters, and fill the values of ```BookmarkTitle```, ```BookmarkLevel``` and ```BookmarkPageNumber``` as per every chapter's infomation. Here is what I have done with the three first chapters of Java Cookbook:

```
BookmarkBegin
BookmarkTitle: Chapter 1. Getting Started: Compiling, Running, and Debugging
BookmarkLevel: 1
BookmarkPageNumber: 22
BookmarkBegin
BookmarkTitle: Chapter 2. Interacting with the Environment
BookmarkLevel: 1
BookmarkPageNumber: 73
BookmarkBegin
BookmarkTitle: Chapter 3. Strings and Things
BookmarkLevel: 1
BookmarkPageNumber: 91
```

Once all the chapters have been done, we save the text file, and the text file is ready to be imported into the oringinal PDF file. 

### 3. Import meta data to PDF Ebook

Use the following command-line to import meta data that has been saved and updated in the text file back to PDF file.

`pdftk [source pdf] update_info [text file] output [destination pdf]`

Here, we continue our example:

`pdftk ./JavaCookBook.pdf update_info ./bookmark.txt output ./JavaCookBookToC.pdf`

Voilà, We get an ebook with its clickable table of content saved in ```JavaCookBookToc.pdf```!

## Conclusion

PDFtk only has command-line version for Linux, which is a little bit hard to use comparing to GUI version. However, Linux users are not scared of command lines. For more usage, you could type `pdftk --help`.

Enjoy!
