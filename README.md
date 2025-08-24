# imgresize

This repository is a rust program that takes 4 parameters

* infile
* outfile
* the number of pixels wide for the outfile
* the filtering (Default  `"lanczos"` but something — anything — must be provided)

It can be invoked with a batch file as follows for example.  This will take
all the `.jpg` files in `./originals` and compress those into subfolders
like `./originals/821`, `./originals/614`, etc.

```bash
#!/bin/bash
# filepath: ./batch_resize.sh

set -e

SOURCEDIR="./originals"
SIZES=(1000 500 250)

find "$SOURCEDIR" -maxdepth 1 -type f -iname '*.jpg' | while read -r srcfile; do
    filename=$(basename "$srcfile")
    for size in "${SIZES[@]}"; do
        outdir="$SOURCEDIR/$size"
        # mkdir -p "$outdir"
        outfile="$outdir/$filename"
        if [[ -f "$outfile" ]]; then
            # echo "Skipping $outfile (already exists)"
            echo "Skipping $outfile"
        else
            echo "Resizing $srcfile to width $size"
            imgresize "$srcfile" "$outfile" "$size" whatever
            echo "Created $outfile"
        fi
    done
done
```
