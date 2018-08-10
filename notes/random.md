- [Extract text from PDF](#extract-text-from-pdf)
    - [Tools Tried:](#tools-tried)
        - [pypdf2](#pypdf2)
        - [pdfminder](#pdfminder)
        - [pdftotext](#pdftotext)


# Extract text from PDF

## Tools Tried:

Following are based on my attempts to extract phenotype data from phenotips file.  These may not necessarily be true/apply for other pdf files.

### pypdf2

Python module. Failed to extract most of the text. Extracted just small part of pdf.


### pdfminder

* Command line tool
* Required installation from source.
* Worked reasonably well. Seemed to extract most of the text, if not all.
* Has -layout_mode option to preserve the layout of text as in pdf. Haven't tried though.
* Doesn't seem to allow ignoring header/footer though


### pdftotext

* Command line tool

* Installed in mac - "brew install poppler". It's part of poppler suite.
    ```
    # simple conversion
    pdftotext sample.pdf out.txt
    
    # to keep layout of text as in input pdf
    pdftotext -layout sample.pdf out.txt
    
    # to change encoding. Default UTF-8 encoding resulted in 'fi' as one letter instead of twletters. ASCII7 worked out well.
    pdftotext -layout enc ASCII7 sample.pdf out.txt
    ```

* Recognizing header and footer is not straightforward but can be done by choosing the area of pdthat need to be converted. See this stackoverflow answer.

* What worked for me in my case for phenotips file

    ```
    pdftotext -layout enc ASCII7 -y 80 -H 640 -W 1000 sample.pdf out.txt
    ```
