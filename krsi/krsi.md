# KRSI LSM + BPF

eBPF programs that use **BPF Type Format (BTF)** do not need to include kernel headers for accessing information from the attached eBPF program’s context. *They can simply declare the structures in the eBPF program and only specify the fields that need to be accessed.*

```c
struct mm_struct {
        unsigned long start_brk, brk, start_stack;
} __attribute__((preserve_access_index));
```

Anyway, this can be further simplified (if one has access to the BTF information at build time) by generating the `vmlinux.h` with:

```shell
❯ bpftool btf dump file /sys/kernel/btf/vmlinux format c > vmlinux.h
```

The `vmlinux.h` can then simply be included in the BPF programs without requiring the definition of the types.
