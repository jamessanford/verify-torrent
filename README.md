##Show completed files from a .torrent file.

###Print out completed files (those with valid checksums), like 'find':
```
% verify-torrent file.torrent
```

###Set aside only the completed files for analysis:

#####First, duplicate the directory tree into 'save':
```
% find torrentdir -type d -print0 | xargs -0 -n 1 -I I mkdir -p save/I
```

#####Then, hard link the completed files into 'save':
```
% verify-torrent -0 file.torrent | xargs -0 -n 1 -I I ln I save/I
```

#####Finally, you can **rm -rf** the incomplete 'torrentdir'.
#####Or, if the transfer is still in progress, you can rerun the verify pipeline.

> ###Operational Caveat:
>  You should leave the incomplete torrent files intact before running this.
>
>  To know if a file is valid, the entire surrounding piece must be valid,
>  and this piece likely covers multiple files.  If you remove known incomplete
>  files, this piece data will be unavailable for checksum and now you cannot
>  determine the validity of the rest of the files covered by the piece.
