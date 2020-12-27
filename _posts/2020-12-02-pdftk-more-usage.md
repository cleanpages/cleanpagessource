---
title: "More usage of pdftk"
excerpt: "merge, split, decrypt PDF and more..."
author_profile: false
categories:
  - PDF
tags:
  - PDFtk
Layout: posts
toc: false
toc_sticky: false
--- 
Here is more usage of pdftk. This post keeps being updated...

**Note**: If you want to know how to install pdftk in Linux, please refer to
[Create a table of content with links for PDF](https://cleanpages.github.io/website/pdf/add-pdf-table-of-content/)
{: .notice--warning}

### Merge multiple PDF files to one PDF files
* Change into the directory where the PDF files are waiting to merge, then type:
```
pdftk *.pdf cat output bind.pdf
```
* Merge PDFs with a specified order:
```
pdftk A=file1.pdf B=file2.pdf cat A B output bind12.pdf
```

### Split PDF pages into single page PDFs
The following command splits `bind.pdf` into single page PDFs as `page01.pdf`, `page02.pdf`, `page03.pdf`...
```
pdftk bind.pdf burst output ./pdfs/page%2d.pdf
```
if the `output` section is omitted, the single page PDFs will generated in the current directory.

### Delete pages in a PDF file
For example, Remove page 9 from file.pdf, and save in new.pdf:
```
pdftk A=file.pdf cat A1-8 A10-end output new.pdf
```
### Set a password to protect PDF
```
pdftk file.pdf output protected.pdf owner_pw <password>
```

### Open a protected PDF with its preset password
```
pdftk file.pdf input-pw <password> output unsecure.pdf
```
**Note**: This command seems not functional with the PDF that is encrypted by Adobe Acrobat.
{: .notice--warning}





