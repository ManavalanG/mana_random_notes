## Extract text from PDF

### Tools Tried

Following are based on my attempts to extract phenotype data from phenotips file.  These may not necessarily be true/apply for other pdf files.

#### pypdf2

Python module. Failed to extract most of the text. Extracted just small part of pdf.


#### pdfminder

- Command line tool
- Required installation from source.
- Worked reasonably well. Seemed to extract most of the text, if not all.
- Has -layout_mode option to preserve the layout of text as in pdf. Haven't tried though.
- Doesn't seem to allow ignoring header/footer though


#### pdftotext

- Command line tool

- Installed in mac - "brew install poppler". It's part of poppler suite.
  
    ```sh
    # simple conversion
    pdftotext sample.pdf out.txt
    
    # to keep layout of text as in input pdf
    pdftotext -layout sample.pdf out.txt
    
    # to change encoding. Default UTF-8 encoding resulted in 'fi' as one letter instead of twletters. ASCII7 worked out well.
    pdftotext -layout enc ASCII7 sample.pdf out.txt
    ```

- Recognizing header and footer is not straightforward but can be done by choosing the area of pdf tthat need to be converted. See this [stackoverflow answer](https://stackoverflow.com/a/35005347/3998252).

- What worked for me in my case for phenotips file

    ```sh
    pdftotext -layout enc ASCII7 -y 80 -H 640 -W 1000 sample.pdf out.txt
    ```

## Convert Excel xlsx to csv

- [xlsx2csv](https://github.com/dilshod/xlsx2csv)
    - Easy to install; Worked well in my try.
- [More tools](https://stackoverflow.com/questions/10557360/convert-xlsx-to-csv-in-linux-with-command-line)


## UCSC genome browser

### Interesting features

#### See settings used in text format

Either of these would do:

- Replace existing url with http://genome.ucsc.edu/cgi-bin/cartDump to see the settings used in text format
- Save settings file via `my data` -> `my session` -> `save settings`


#### Short match

BLAT is useful is seq is >20 bases. For shorted sequences (2-30 bases), [short match](http://genome.ucsc.edu/cgi-bin/hgTrackUi?hgsid=711072351_fyVRofGkapbguvAvVWR9j2gJjgoN&c=chr16&g=oligoMatch) should work better.


#### Show DNA seqs in color based on features

[Source](https://genome.ucsc.edu/goldenpath/help/hgTracksHelp.html#TrackFormatDNA)

Can be used to color and toggle case for sections of DNA (exon, SNP, etc.)

#### Primer design

[Source](http://genome.ucsc.edu/cgi-bin/hgPcr)

#### Save snapshots

* `View` -> `PDF/PS`
* Change `hgTracks` to `hgRenderTracks` in url to get the png output
    http://genome.ucsc.edu/cgi-bin/hgRenderTracks?parameters

