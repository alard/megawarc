Megawarc
========
megawarc is useful if you have .tar full of .warc.gz files and you really want one big .warc.gz. With megawarc you get your .warc.gz, but you can still restore the original .tar.

The megawarc tool looks for .warc.gz in the .tar file and creates three files, the megawarc:
* FILE.warc.gz   is the concatenated .warc.gz
* FILE.tar       contains any non-warc files from the .tar
* FILE.json.gz   contains metadata

You need the JSON file to reconstruct the original .tar from
the .warc.gz and .tar files. The JSON file has the location
of every file from the original .tar file.


Metadata format
---------------
One line with a JSON object per file in the .tar.
```js
{
   "target": {
     "container": "warc" or "tar", // where is this file?
     "offset": number,             // where in the tar/warc does this file start?
                                   // for files in the tar this includes the tar header, which is
                                   // copied to the tar.
     "size": size                  // where does this file end?
                                   // for files in the tar, this includes the padding to 512 bytes
   },
   "src_offsets": {
     "entry": number,              // where is this file in the original tar?
     "data": number,               // where does the data start? entry+512
     "next_entry": number          // where does the next tar entry start
   },
   "header_fields": {
     ...                           // parsed fields from the tar header
   },
   "header_string": string         // the tar header for this entry
}
```

Usage
-----
```
megawarc build FILE
```

Converts the tar file (containing .warc.gz files) to a megawarc.
It creates FILE.warc.gz, FILE.tar and FILE.json.gz from FILE.

```
megawarc restore FILE
```

Converts the megawarc back to the original tar.
It reads FILE.warc.gz, FILE.tar and FILE.json.gz to make FILE.

