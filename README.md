I downloaded control ultimate edition using both tools (https://github.com/Sude-/lgogdownloader/tree/master) and (https://github.com/Kalanyr/gogrepoc).
When doing the initial library scan `lgogdownloader --list` compared to `python3 gogrepoc.py update`  gogrepoc was much slower parsing the library data (i have not looked into what the tools exactly do so it might not be identical) 
but lgogdownloader was a few seconds compared to ~5 minutes for gogrepoc to parse the library (again i am not sure they are doing exactly the same). The download test i performed was done using 4 threads for each of them,
I also did a diff after the download where the only difference was gogrepoc downloading metadata and the different folder structure for how they save the patches, but both downloaded the patch. Here is the first diff:
 ```
$ diff lgogdownloader-3.12/control_ultimate_edition/ gogrepoc/control_ultimate_edition/
Only in gogrepoc/control_ultimate_edition/: !images
Only in gogrepoc/control_ultimate_edition/: !info.txt
Only in gogrepoc/control_ultimate_edition/: patch_control_ultimate_edition_Update_1_(41028)_to_Update_2_(41492).exe
Only in lgogdownloader-3.12/control_ultimate_edition/: patches
```
Second diff after doing 
`$ mv lgogdownloader-3.12/control_ultimate_edition/patches/patch_control_ultimate_edition_Update_1_\(41028\)_to_Update_2_\(41492\).exe lgogdownloader-3.12/control_ultimate_edition/`
and `rm -r lgogdownloader-3.12/control_ultimate_edition/patches/`: 
```
$ diff lgogdownloader-3.12/control_ultimate_edition/ gogrepoc/control_ultimate_edition/
Only in gogrepoc/control_ultimate_edition/: !images
Only in gogrepoc/control_ultimate_edition/: !info.txt
```

Here is the download timed:
lgogdownloader:
```
$ time lgogdownloader --download --game control_ultimate_edition
Getting game names (2/2) 95 / 95
Getting game info 1 / 1
Total size: 26.19 GB
2024-Apr-24 20:00:36 [Thread #0] Download complete: setup_control_ultimate_edition_update_2_(41492).exe (@ 2.88MB/s)
2024-Apr-24 20:12:32 [Thread #3] Download complete: setup_control_ultimate_edition_update_2_(41492)-3.bin (@ 5.71MB/s)
2024-Apr-24 20:12:33 [Thread #1] Download complete: setup_control_ultimate_edition_update_2_(41492)-1.bin (@ 5.71MB/s)
2024-Apr-24 20:12:33 [Thread #2] Download complete: setup_control_ultimate_edition_update_2_(41492)-2.bin (@ 5.71MB/s)
2024-Apr-24 20:12:33 [Thread #0] Download complete: setup_control_ultimate_edition_update_2_(41492)-4.bin (@ 5.71MB/s)
2024-Apr-24 20:12:59 [Thread #0] Download complete: patch_control_ultimate_edition_Update_1_(41028)_to_Update_2_(41492).exe (@ 5.68MB/s)
2024-Apr-24 20:12:59 [Thread #0] Finished all tasks
2024-Apr-24 20:17:20 [Thread #2] Download complete: setup_control_ultimate_edition_update_2_(41492)-7.bin (@ 7.37MB/s)
2024-Apr-24 20:17:20 [Thread #2] Finished all tasks
2024-Apr-24 20:20:08 [Thread #3] Download complete: setup_control_ultimate_edition_update_2_(41492)-5.bin (@ 9.03MB/s)
2024-Apr-24 20:20:08 [Thread #3] Finished all tasks
2024-Apr-24 20:20:08 [Thread #1] Download complete: setup_control_ultimate_edition_update_2_(41492)-6.bin (@ 9.02MB/s)
2024-Apr-24 20:20:08 [Thread #1] Finished all tasks
#0: Finished
#1: Finished
#2: Finished
#3: Finished

real19m37.732s
user0m59.304s
sys1m23.422s
```

gogrepoc (i have the full output but it is ~5000 lines):
```
$ time python3 gogrepoc.py download -ids control_ultimate_edition -os windows > control-test.txt
real18m12.346s
user8m29.640s
sys4m27.027s
```
