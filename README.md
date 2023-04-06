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
