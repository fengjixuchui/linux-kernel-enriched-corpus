# Linux Kernel Enriched Corpus for Fuzzers

Documentation for using and generating the Enriched corpus provided here.

For more questions, feel free to email [Palash Oswal](https://oswalpalash.com) or [Rohan Padhye](https://rohan.padhye.org).

## Using Enriched corpus with Syzkaller

The latest copy of the Corpus file [corpus.db](./corpus.db) is available in this repository. The file is updated daily.

Download it to [syzkaller](https://github.com/google/syzkaller) workdir and start syzkaller.
```
mkdir workdir
cd workdir
wget https://github.com/cmu-pasta/linux-kernel-regression-corpus/raw/main/corpus.db
```

## Using Enriched corpus with HEALER

The corpus programs are stored in `files` directory and can directly be imported to [HEALER](https://github.com/SunHao-0/healer).

Clone the repository and copy over `files/*` to `output/corpus/` directory in HEALER. From within HEALER working directory, run the following commands.
```
mkdir -p output/corpus
cp -vr <path/to/files/> output/corpus/
```

### Fetching Corpus Manually

[collect.py](./collect.py) : currently fetches `syz` reproducers from all fixed Linux Kernel upstream crashes in [syzbot](syzkaller.appspot.com/upstream/fixed).

This script can be modified to fetch corpus programs from other kernel versions and to fetch "C" Programs instead of `syz` reproducers.

### Generating corpus.db File

If you have a collection of `syz` programs that need to be converted to a syzkaller comptaible `corpus.db` file, you can use `syz-db.go pack` from syzkaller.

An implementation of this is available in the GitHub actions workflow [here](./.github/workflows/corpusgen.yml).


## Results

Experiments performed by fuzzing 1 instance using 2VCPUs and 4GB RAM for 24 hours.

System Used : [ThinkMate](https://www.thinkmate.com/system/rax-xf2-11s1-sh), Intel® Xeon® Gold 6226R.

Kernel Versions Tested:  Linux v6.0.8 and v6.1.20

### Coverage over time 
![image](https://user-images.githubusercontent.com/6431196/230692263-cb912a13-6f63-4eaa-8e4f-78ce27f011a3.png)
![image](https://user-images.githubusercontent.com/6431196/230692906-62701966-2873-424a-ae71-100bbc8529b1.png)

### Unique Crashes over time
![image](https://user-images.githubusercontent.com/6431196/230692399-c1b59af0-8c15-4461-9c8f-94eedd26e07d.png)

### Total Crashes over time
![image](https://user-images.githubusercontent.com/6431196/230692326-b172512d-0335-4b13-bacf-790438ea5ead.png)

### CVEs:
* [CVE-2023-26544](https://www.cve.org/CVERecord?id=CVE-2023-26544)
* [CVE-2023-26605](https://www.cve.org/CVERecord?id=CVE-2023-26605)
* [CVE-2023-26606](https://www.cve.org/CVERecord?id=CVE-2023-26606)
* [CVE-2023-26607](https://www.cve.org/CVERecord?id=CVE-2023-26607)

### New Bugs Reported:
* https://lkml.org/lkml/2023/2/20/128
* https://lkml.org/lkml/2023/2/20/773
* https://lkml.org/lkml/2023/2/20/785
* https://lkml.org/lkml/2023/2/20/860
* https://lkml.org/lkml/2023/2/21/1353
* https://lkml.org/lkml/2023/2/22/3 
[and many more](https://twitter.com/oswalpalash/status/1627776397828853760)

### More bugs discovered (includes bugs that were found sooner than syzbot & bugs undiscovered by syzbot)

| Title                                                     | Found in #Instance | Date of Discovery | Branch (if found by syzbot)| New/Earlier |
|-----------------------------------------------------------|--------------------|-------------------|--------------------|------------------|
| UBSAN: shift-out-of-bounds in ntfs_fill_super              | 10                 | 2/28/23           | 6.2.0              | Yes              |
| UBSAN: shift-out-of-bounds in nilfs_load_super_block       | 10                 | 10/25/22          | net-6.1-rc3-1       | Yes              |
| UBSAN: shift-out-of-bounds in dbAllocAG                    | 10                 | 9/28/22           | 6.0.0-rc7          | Yes              |
| KASAN: use-after-free Read in si470x_int_in_callback       | 10                 | N/A               | regression         | Yes              |
| KASAN: use-after-free Read in run_unpack                   | 10                 | N/A               | new                | Yes              |
| KASAN: use-after-free Read in ntfs_trim_fs                  | 10                 | N/A               | new                | Yes              |
| KASAN: slab-out-of-bounds Read in hdr_find_e                | 10                 | N/A               | new                | Yes              |
| KASAN: out-of-bounds Read in leaf_paste_entries             | 10                 | N/A               | regression         | Yes              |
| KASAN: null-ptr-deref Write in f2fs_stop_discard_thread     | 10                 | N/A               | new                | Yes              |
| KASAN: slab-out-of-bounds Read in ntfs_attr_find             | 8                  | N/A               | new/regression     | Yes              |
| KASAN: use-after-free Read in em28xx_init_extension         | 6                  | 3/30/22           | 5.17.0-syzkaller-   | Yes              |
| KASAN: use-after-free Read in do_garbage_collect            | 6                  | 11/13/22          | 6.1.0-rc4-syzkaller | Yes              |
| KASAN: slab-out-of-bounds Read in do_garbage_collect         | 6                  | 11/13/22          | 6.1.0-rc4-syzkaller | Yes              |
| KASAN: use-after-free Read in cfusbl_device_notify           | 5                  | 11/12/22          | 5.18.0-rc1-syzkaller| Yes              |
| KASAN: use-after-free Read in notifier_call_chain            | 4                  | 11/18/22          | 5.18.0-rc3-         | Yes              |
| KASAN: use-after-free Write in nr_release                    | 3                  | N/A               | regression         | Yes              |
| KASAN: use-after-free Read in task_work_run                  | 2                  | N/A               | new                | Yes              |
| KASAN: use-after-free Read in inode_cgwb_move_to_attached    | 2                  | N/A               | new                | Yes              |
| KASAN: use-after-free Read in __fib6_clean_all               | 2                  | N/A               | new                | Yes              |
| KASAN: use-after-free Read in tcp_retransmit_timer           | 1                  | N/A               | regression         | Yes              |
| KASAN: use-after-free Read in nexthop_flush_dev              | 1                  | N/A               | new                | Yes              |
| KASAN: use-after-free Read in lock_sock_nested               | 1                  | 9/1/20            | 5.9.0-rc3          | Yes              |
| KASAN: slab-out-of-bounds Read in mi_enum_attr               | 1                  | N/A               | new                | Yes              |
