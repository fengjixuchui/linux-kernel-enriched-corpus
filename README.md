# Linux Kernel Regression Corpus for Fuzzers

Documentation for using and generating the corpus provided here.

For more questions, feel free to email [Palash Oswal](https://oswalpalash.com) or [Rohan Padhye](https://rohan.padhye.org).

## Using corpus with Syzkaller

The latest copy of the Corpus file [corpus.db](./corpus.db) is available in this repository. The file is updated daily.

Download it to [syzkaller](https://github.com/google/syzkaller) workdir and start syzkaller.
```
mkdir workdir
cd workdir
wget https://github.com/cmu-pasta/linux-kernel-regression-corpus/raw/main/corpus.db
```

## Using corpus with HEALER

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

New Bugs Reported:
* https://lkml.org/lkml/2023/2/20/128
* https://lkml.org/lkml/2023/2/20/773
* https://lkml.org/lkml/2023/2/20/785
* https://lkml.org/lkml/2023/2/20/860
* https://lkml.org/lkml/2023/2/21/1353
* https://lkml.org/lkml/2023/2/22/3 
[and many more](https://twitter.com/oswalpalash/status/1627776397828853760)

Exhaustive List of Bugs Discovered with Syzkaller using Regression Corpus:
* WARNING in udf_free_inode
* WARNING in tcp_enter_loss
* WARNING in shark_write_val/usb_submit_urb
* WARNING in send_packet/usb_submit_urb
* WARNING in notify_change
* WARNING in nilfs_sufile_set_segment_usage
* WARNING in nilfs_dat_prepare_end
* WARNING in nilfs_dat_commit_end
* WARNING in iomap_iter
* WARNING in inc_nlink
* WARNING in hif_usb_send/usb_submit_urb
* WARNING in ext4_xattr_block_set
* WARNING in bpf_check
* WARNING in ar5523_cmd/usb_submit_urb
* WARNING in __dev_queue_xmit
* WARNING in __brelse
* UBSAN: shift-out-of-bounds in snto32
* UBSAN: shift-out-of-bounds in ntfs_fill_super
* UBSAN: shift-out-of-bounds in nilfs_load_super_block
* UBSAN: shift-out-of-bounds in init_sb
* UBSAN: shift-out-of-bounds in dbAllocAG
* suppressed report
* possible deadlock in virtual_nci_close
* possible deadlock in rfcomm_sk_state_change
* possible deadlock in nilfs_count_free_blocks
* possible deadlock in nci_start_poll
* possible deadlock in btrfs_dirty_inode
* kernel BUG in gfs2_fill_super
* KASAN: use-after-free Read in si470x_int_in_callback
* KASAN: use-after-free Read in run_unpack
* KASAN: use-after-free Read in ntfs_trim_fs
* KASAN: use-after-free Read in ntfs_attr_find
* KASAN: use-after-free Read in hdr_find_e
* KASAN: use-after-free Read in drm_gem_object_release_handle
* KASAN: use-after-free Read in ar5523_cmd_tx_cb
* KASAN: slab-out-of-bounds Write in udf_find_entry
* KASAN: slab-out-of-bounds Read in run_unpack
* KASAN: slab-out-of-bounds Read in ntfs_trim_fs
* KASAN: slab-out-of-bounds Read in hdr_find_e
* KASAN: out-of-bounds Read in leaf_paste_entries
* KASAN: null-ptr-deref Write in f2fs_stop_discard_thread
* INFO: task hung in nfnetlink_rcv_msg
* INFO: task hung in ip_set_net_exit
* INFO: task hung in blkdev_put
* INFO: rcu detected stall in corrupted
* general protection fault in nilfs_segctor_do_construct
* general protection fault in ni_write_inode
* general protection fault in end_page_writeback
* general protection fault in dbgfs_rm_context_write
* general protection fault in can_rcv_filter
* general protection fault in __queue_work
* BUG: unable to handle kernel paging request in mi_enum_attr
* BUG: sleeping function called from invalid context in kernfs_walk_and_get_ns
* BUG: Bad page state
* WARNING: ODEBUG bug in virtual_ncidev_close
* WARNING in nf_tables_exit_net
* WARNING in btrfs_commit_transaction
* WARNING in btf_type_id_size
* WARNING in __skb_flow_dissect
* possible deadlock in input_event
* possible deadlock in btrfs_commit_transaction
* possible deadlock in __nilfs_error
* kernel BUG in ext4_write_inline_data
* general protection fault in skb_release_data
* general protection fault in em_u32_match
* general protection fault in em_cmp_match
* general protection fault in d_flags_for_inode
* BUG: unable to handle kernel paging request in ntfs_attr_find
* BUG: unable to handle kernel paging request in hdr_find_e
* BUG: stack guard page was hit in inet6_release
* WARNING in nci_unregister_device
* kernel BUG in pfkey_send_acquire
* KASAN: use-after-free Read in move_expired_inodes
* KASAN: slab-out-of-bounds Read in ntfs_attr_find
* general protection fault in nilfs_palloc_commit_free_entry
* KASAN: use-after-free Read in __mark_inode_dirty
* WARNING in __perf_event_overflow
* KASAN: use-after-free Read in mi_enum_attr
* KASAN: use-after-free Read in em28xx_init_extension
* KASAN: use-after-free Read in do_garbage_collect
* KASAN: slab-out-of-bounds Read in do_garbage_collect
* INFO: rcu detected stall in do_idle
* WARNING in j1939_session_deactivate
* WARNING in hugetlb_wp
* KASAN: use-after-free Read in cfusbl_device_notify
* INFO: rcu detected stall in tc_modify_qdisc
* possible deadlock in sco_connect_cfm
* KASAN: use-after-free Read in notifier_call_chain
* INFO: task hung in p9_fd_close
* WARNING in rose_device_event
* WARNING in input_unregister_device
* WARNING in __udf_add_aext
* possible deadlock in p9_req_put
* possible deadlock in __btrfs_tree_lock
* KASAN: use-after-free Write in nr_release
* INFO: rcu detected stall in newlstat
* INFO: rcu detected stall in net_tx_action
* WARNING in schedule_bh
* unregister_netdevice: waiting for DEV to become free
* possible deadlock in sco_conn_del
* possible deadlock in jbd2_journal_lock_updates
* possible deadlock in ext4_bmap
* KASAN: use-after-free Read in task_work_run
* KASAN: use-after-free Read in inode_cgwb_move_to_attached
* KASAN: use-after-free Read in __fib6_clean_all
* INFO: task hung in nfc_rfkill_set_block
* INFO: rcu detected stall in syscall_exit_to_user_mode
* INFO: rcu detected stall in clone
* UBSAN: shift-out-of-bounds in extAlloc
* possible deadlock in btrfs_search_slot
* possible deadlock in __jbd2_log_wait_for_space
* KFENCE: use-after-free in drm_gem_object_release_handle
* kernel BUG in fou_build_udp
* kernel BUG in do_journal_end
* KASAN: use-after-free Read in tcp_retransmit_timer
* KASAN: use-after-free Read in nilfs_segctor_sync
* KASAN: use-after-free Read in nexthop_flush_dev
* KASAN: use-after-free Read in lock_sock_nested
* KASAN: slab-out-of-bounds Read in mi_enum_attr
* INFO: task hung in usbdev_release
* INFO: task hung in ip_set_nfnl_get_byindex
* INFO: task hung in blkdev_fallocate
* INFO: rcu detected stall in sys_symlink
* INFO: rcu detected stall in sys_rename
* INFO: rcu detected stall in smp_call_function
* INFO: rcu detected stall in pcpu_balance_workfn
* INFO: rcu detected stall in newfstat
* INFO: rcu detected stall in ip_rcv
* INFO: rcu detected stall in ext4_file_write_iter
* INFO: rcu detected stall in do_sys_open
* INFO: rcu detected stall in cleanup_net
* INFO: rcu detected stall in addrconf_dad_work
* INFO: rcu detected stall in __netif_receive_skb_core
* INFO: rcu detected stall in __fput
* general protection fault in skb_unlink
* general protection fault in requeue_rx_msgs
* general protection fault in prepare_to_wait
* general protection fault in locked_inode_to_wb_and_lock_list
* BUG: soft lockup in smp_call_function

Exhaustive List of Bugs Discovered with HEALER using Regression Corpus:
* BUG: corrupted list in em28xx_init_extension
* BUG: unable to handle kernel paging request in eventfd_ctx_put
* general protection fault in prepare_to_wait
* KASAN: slab-out-of-bounds Read in ntfs_attr_find
* KASAN: slab-out-of-bounds Read in print_report
* KASAN: use-after-free Read in ar5523_cmd_tx_cb
* KASAN: use-after-free Read in cfusbl_device_notify
* KASAN: use-after-free Read in drm_gem_object_release_handle
* KASAN: use-after-free Read in __fib6_clean_all
* KASAN: use-after-free Read in notifier_call_chain
* KASAN: use-after-free Read in ntfs_attr_find
* KASAN: use-after-free Read in print_report
* KASAN: use-after-free Read in si470x_int_in_callback
* KASAN: use-after-free Read in tcp_retransmit_timer
* kernel BUG in do_journal_end
* kernel panic: panic_on_warn set
* possible deadlock in jbd2_journal_lock_updates
* possible deadlock in p9_req_put
* possible deadlock in rfcomm_sk_state_change
* unregister_netdevice: waiting for DEV to become free
* WARNING in ar5523_cmd~usb_submit_urb
* WARNING in ath9k_hif_usb_alloc_urbs~usb_submit_urb
* WARNING in hif_usb_send~usb_submit_urb
* WARNING in j1939_session_deactivate
* WARNING in __perf_event_overflow
* WARNING in shark_write_val~usb_submit_urb
* WARNING in __skb_flow_dissect
* WARNING in tcp_enter_loss
