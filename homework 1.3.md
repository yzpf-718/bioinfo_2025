#### read.sh
```
#!/bin/bash
> filenames.txt
> dirname.txt
FolderPath="./bash_homework/"
DIR=$(ls -1 "$FolderPath")
for val in $DIR
do
full_path="$FolderPath$val"
if [ -f "$full_path" ]; then
echo -e "FILE: $val\n" >> filenames.txt
elif [ -d "$full_path" ]; then
echo -e "DIR: $val\n" >> dirname.txt
fi
done
exit 0
```
#### dirname.txt
DIR: a-docker

DIR: app

DIR: backup

DIR: bin

DIR: biosoft

DIR: c1-RBPanno

DIR: datatable

DIR: db

DIR: download

DIR: e-annotation

DIR: exRNA

DIR: genome

DIR: git

DIR: highcharts

DIR: home

DIR: hub29

DIR: ibme

DIR: l-lwl

DIR: map2

DIR: mljs

DIR: module

DIR: mogproject

DIR: node_modules

DIR: perl5

DIR: postar2

DIR: postar_app

DIR: postar.docker

DIR: RBP_map

DIR: rout

DIR: script

DIR: script_backup

DIR: software

DIR: tcga

DIR: test

DIR: tmp

DIR: tmp_script

DIR: var

DIR: x-rbp

#### filenames.txt
FILE: a1.txt

FILE: a.txt

FILE: b1.txt

FILE: bam_wig.sh

FILE: b.filter_random.pl

FILE: c1.txt

FILE: chrom.size

FILE: c.txt

FILE: d1.txt

FILE: dir.txt

FILE: e1.txt

FILE: f1.txt

FILE: human_geneExp.txt

FILE: if.sh

FILE: image

FILE: insitiue.txt

FILE: mouse_geneExp.txt

FILE: name.txt

FILE: number.sh

FILE: out.bw

FILE: random.sh

FILE: read.sh

FILE: test3.sh

FILE: test4.sh

FILE: test.sh

FILE: test.txt

FILE: wigToBigWig

