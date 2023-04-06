# Linux Kernel Regression Corpus for Fuzzers

Documentation for using and generating the corpus provided here.

For more questions, feel free to email [Palash Oswal](https://oswalpalash.com) or [Rohan Padhye](https://rohan.padhye.org).

## Using corpus with Syzkaller

The latest copy of the Corpus file [corpus.db](./corpus.db) is available in this repository. The file is updated daily. 
Download it to [syzkaller](https://github.com/google/syzkaller) workdir and start syzkaller.
`mkdir workdir`
`cd workdir`
`wget https://github.com/cmu-pasta/linux-kernel-regression-corpus/raw/main/corpus.db`
Start Fuzzer

## Using corpus with HEALER

The corpus programs are stored in `files` directory and can directly be imported to [HEALER](https://github.com/SunHao-0/healer).
Clone the repository and copy over `files/*` to `output/corpus/` directory in HEALER. From within HEALER working directory, run the following commands.
`mkdir -p output/corpus`
`cp -vr <path/to/files/> output/corpus/`
Start Fuzzer

### Fetching Corpus Manually

[collect.py](./collect.py) : currently fetches `syz` reproducers from all fixed Linux Kernel upstream crashes in [syzbot](syzkaller.appspot.com/upstream/fixed).
This script can be modified to fetch corpus programs from other kernel versions and to fetch "C" Programs instead of `syz` reproducers.

### Generating corpus.db File

If you have a collection of `syz` programs that need to be converted to a syzkaller comptaible `corpus.db` file, you can use `syz-db.go pack` from syzkaller.
An implementation of this is available in the GitHub actions workflow [here](./.github/workflows/corpusgen.yml).


## Results

Coverage over time 
![image](https://user-images.githubusercontent.com/6431196/230491976-e58db401-d572-4c58-ab18-59cbcf4031ec.png)

Unique Crashes over time
![image](https://user-images.githubusercontent.com/6431196/230492537-d1d859a3-6b4a-432e-a290-44fcc5913cd1.png)

Total Crashes over time
![image](https://user-images.githubusercontent.com/6431196/230493115-42fe4205-06b3-471a-b228-426003d1e3b7.png)

CVEs:
* [CVE-2023-26544](https://www.cve.org/CVERecord?id=CVE-2023-26544)
* [CVE-2023-26605](https://www.cve.org/CVERecord?id=CVE-2023-26605)
* [CVE-2023-26606](https://www.cve.org/CVERecord?id=CVE-2023-26606)
* [CVE-2023-26607](https://www.cve.org/CVERecord?id=CVE-2023-26607)

Bugs:
* https://lkml.org/lkml/2023/2/20/128
* https://lkml.org/lkml/2023/2/20/773
* https://lkml.org/lkml/2023/2/20/785
* https://lkml.org/lkml/2023/2/20/860
* https://lkml.org/lkml/2023/2/21/1353
* https://lkml.org/lkml/2023/2/22/3 
[and many more](https://twitter.com/oswalpalash/status/1627776397828853760)
