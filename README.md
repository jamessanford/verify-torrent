##Show completed files from a .torrent file.

###Print out completed files (those with valid checksums), like 'find':
```
% verify-torrent file.torrent
```

###Create hard links to all the completed files:
```
  % verify-torrent --link=SAVE file.torrent
```

You can run this while the torrent is in progress.
The `SAVE` directory will contain only the completed files.
Rerun the command to link any new files that have been completed.

If the torrent is no longer in progress, it is now safe to delete the
original incomplete directory and just keep the `SAVE` directory.

###Generate a `--select-file` command line to seed only completed files:
```
  % verify-torrent -s file.torrent
  --select-file=3,10,13
```

###To do `--link` by hand as a shell pipeline:

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
>  Leave the incomplete torrent files intact before running verify-torrent.
>
>  To know if a file is valid, the entire surrounding piece must be valid,
>  and this piece likely covers multiple files.  If you remove known incomplete
>  files, this piece data will be unavailable for checksum and now you cannot
>  determine the validity of the rest of the files covered by the piece.
