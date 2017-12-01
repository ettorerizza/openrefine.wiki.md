_How to import data into OpenRefine to create new projects._

To use OpenRefine on some data, you must first import it into Refine. It's like uploading a data file into, say, Google Spreadsheets, except that there is no **up** in uploading, because Refine runs on your own computer anyway. This importing process creates a new Refine project for your data file.

OpenRefine understands a variety of data file formats. Currently, it tries to guess the format based on the file extension. For example, <tt>.xml</tt> files are of course in XML. By default, an unknown file extension is assumed to be either tab-separated value (TSV) or comma-separated value (CSV). OpenRefine looks for a tab character; if one is found, it assumes TSV.

The formats currently supported (in version 2.7) include:

- TSV, CSV, or values separated by a custom separator you specify
- Line-based text files
- Fixed-width field text files
- PC-Axis text files
- MARC files
- Excel (.xls, xlsx)
- Open Document Format spreadsheets (.ods)
- XML, RDF as XML
- JSON
- Google Spreadsheets
- RDF N3 triples

If you have data in a very peculiar text format, just import it without splitting lines into columns, and then once it's imported, do your own custom column splitting.

If you upload an archive file (with extension <tt>.zip</tt>, <tt>.tar.gz</tt>, <tt>.tgz</tt>, <tt>.tar.bz2</tt>, <tt>.gz</tt>, or <tt>.bz2</tt>), OpenRefine detects the most common file extension in it and loads all files with that extension into a single project.

You can also point OpenRefine at a URL to a data file or a Google Spreadsheet. The mime-type of that URL tells OpenRefine which format the data is in. Currently only Google Spreadsheets which are published publicly are supported.

Once imported, the data is stored in OpenRefine's own format, and your original data file is left undisturbed.

