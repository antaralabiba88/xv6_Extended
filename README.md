# xv6 Extended: Large File Support

An extension to the x86 version of the xv6 teaching operating system that increases the maximum supported file size by implementing **doubly-indirect block addressing**.

## Problem

The original xv6 filesystem supports files of approximately 140 KB using direct and single-indirect block pointers. This project introduces a doubly-indirect pointer to support files of approximately 65 MB.

## Approach

The implementation extends inode block addressing and updates the filesystem's block mapping and cleanup logic.

- **`fs.h`** — Updated block-addressing constants and the maximum-file-size calculation.
- **`file.h`** — Expanded the inode's block-address array.
- **`fs.c`** — Modified `bmap()` to handle doubly-indirect blocks and `itrunc()` to free allocated data and pointer blocks.
- **`param.h`** — Increased `FSSIZE` to provide sufficient disk space for testing.
- **`Makefile`** — Updated compiler settings and disk-image size.

The project focuses on filesystem functionality without modifying the scheduler or system-call interface.

## Technologies

- C
- xv6-public (x86)
- WSL on Windows
- GCC
- QEMU
- Git and GitHub

## How to Build and Run

Run the following commands in your Linux/WSL terminal from the project directory:

```bash
make clean
make qemu-nox
```

Once xv6 boots, run the following commands at the **xv6 shell prompt**:

```text
stressfs
usertests
```

`stressfs` exercises repeated file writes and reads. `usertests` runs the broader test suite, including large-file operations.

## Results

| Version | Block addressing | Approximate maximum file size |
|---|---|---:|
| Original xv6 | Direct + single-indirect | 140 KB |
| Extended xv6 | Direct + single-indirect + doubly-indirect | 65 MB |

The project report records successful execution of `stressfs` and `usertests`, including the `big files` and `bigwrite` tests, with the final output `ALL TESTS PASSED`.

*Note: These results are taken from the project report and have not been independently rerun while preparing this README.*

## Key Learning Outcomes

- Understanding inode-based filesystem structures.
- Mapping logical file blocks to physical disk blocks through multiple levels of indirection.
- Managing data-block and indirect-block allocation.
- Implementing correct file truncation and resource cleanup.
- Building and testing an operating-system kernel using QEMU and WSL.

## References

- Cox, R., Kaashoek, F., & Morris, R. (2023). *xv6: A Simple, Unix-Like Teaching Operating System*. MIT CSAIL.
- Silberschatz, A., Galvin, P. B., & Gagne, G. (2018). *Operating System Concepts* (10th ed.). Wiley.
- Bach, M. J. (1986). *The Design of the UNIX Operating System*. Prentice Hall.
- McKusick, M. K., Joy, W. N., Leffler, M. C., & Fabry, R. (1984). A fast file system for UNIX. *ACM Transactions on Computer Systems, 2*(3), 181–197.
