+++
title = "This Month in Rust OSDev (March 2021)"
date = 2021-04-05

[extra]
month = "March 2021"
authors = [
    "phil-opp",
    "IsaacWoods",
    # add yourself here
]
+++

Welcome to a new issue of _"This Month in Rust OSDev"_. In these posts, we give a regular overview of notable changes in the Rust operating system development ecosystem.

<!-- more -->

This series is openly developed [on GitHub](https://github.com/rust-osdev/homepage/). Feel free to open pull requests there with content you would like to see in the next issue. If you find some issues on this page, please report them by [creating an issue](https://github.com/rust-osdev/homepage/issues/new) or using our <a href="#comment-form">_comment form_</a> at the bottom of this page.

## Project Updates

In this section, we give an overview of notable changes to the projects hosted under the [`rust-osdev`] organization.

[`rust-osdev`]: https://github.com/rust-osdev/about

### [`bootloader`](https://github.com/rust-osdev/bootloader)

The `bootloader` crate implements a custom Rust-based bootloader for easy loading of 64-bit ELF executables. This month, we fixed some build errors that were caused by the update to LLVM 12 in recent Rust nightlies:

- [Fix linker errors on latest nightlies](https://github.com/rust-osdev/bootloader/pull/139) <span class="gray">(published as `v0.9.15`)</span>
- [Replace all remaining `lea`s with `mov` + `offset`](https://github.com/rust-osdev/bootloader/pull/140) <span class="gray">(published as `v0.9.16`)</span>

We also made some good progress on the [UEFI rewrite](https://github.com/rust-osdev/bootloader/pull/130):

- [Set up VESA mode properly instead of hardcoding it](https://github.com/rust-osdev/bootloader/pull/130/commits/7f7fec78ffb7125a7eb0312698714d7897bf9fb9)
- [Detect actual pixel format instead of hardcoding it (BIOS)](https://github.com/rust-osdev/bootloader/pull/130/commits/58564910a743ec48e6c1b3113151d96c7b54ca63)
- [Use `quote` crate for creating `Config` struct instead of debug impls](https://github.com/rust-osdev/bootloader/pull/130/commits/f7478eba3034c98bde0c7725ce21a7b56a473d61)
- [Allow specifying addresses as TOML integers too](https://github.com/rust-osdev/bootloader/pull/130/commits/ba9d943dbb18ef756979f1d2c14df297c1003b45)
- [Add docs for crate and `Config` struct](https://github.com/rust-osdev/bootloader/pull/130/commits/536e0f6b53b8dcd53b6125c3383dec3bdb7a3cc3)
- [Document the created disk images](https://github.com/rust-osdev/bootloader/pull/130/commits/eccb89d61a3e390b36f767d6d8780187bd962e58)

### [`x86_64`](https://github.com/rust-osdev/x86_64)

The `x86_64` crate provides various abstractions for `x86_64` systems, including wrappers for CPU instructions, access to processor-specific registers, and abstraction types for architecture-specific structures such as page tables and descriptor tables.

In March, we merged these changes:

- [Implement `Clone` for `PageTable`](https://github.com/rust-osdev/x86_64/pull/236) <span class="gray">(published as `v0.13.3`)</span>
- [Implement more fmt traits for addr types](https://github.com/rust-osdev/x86_64/pull/237) <span class="gray">(published as `v0.13.4`)</span>

Thanks to [@toku-sa-n](https://github.com/toku-sa-n) and [@dbeckwith](https://github.com/dbeckwith) for their contributions!

### [`uefi-rs`](https://github.com/rust-osdev/uefi-rs)

The `uefi` crate provides safe and performant wrappers for [UEFI](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface), the successor to the BIOS. In March, we merged the following changes:

- [Expose device path types](https://github.com/rust-osdev/uefi-rs/pull/202)
- [Fix running tests in CI](https://github.com/rust-osdev/uefi-rs/pull/203)
- [Fix typo in regular.rs](https://github.com/rust-osdev/uefi-rs/pull/204)

Thanks to [@gil0mendes](https://github.com/gil0mendes) and [@ocadaruma](https://github.com/ocadaruma) for their contributions!

### [`multiboot2`](https://github.com/rust-osdev/multiboot2-elf64)

The `multiboot2` crate provides abstraction types for the boot information of multiboot2 bootloaders. We merged the following updates this month:

- [Use `impl Iterator` as return type instead of named types](https://github.com/rust-osdev/multiboot2-elf64/pull/72)
- [Docs: Remove fragile `asm!` code example](https://github.com/rust-osdev/multiboot2-elf64/pull/73)
- [Apply `rustfmt`](https://github.com/rust-osdev/multiboot2-elf64/pull/74)

Thanks to [@toku-sa-n](https://github.com/toku-sa-n) for their contributions!

### [`volatile`](https://github.com/rust-osdev/volatile)

The `volatile` crate provides a safe wrapper type for implementing volatile read and write operations. This is useful for accessing memory regions that have side-effects, such as memory-mapped hardware registers or framebuffers. In March, we fixed a build error that was caused by a change in nightly Rust:

- [Replace feature `range_bounds_assert_len` with `slice_range`](https://github.com/rust-osdev/volatile/pull/21) <span class="gray">(published as `v0.4.4`)</span>
- [Add a test for `slice::as_chunks_mut` usage](https://github.com/rust-osdev/volatile/commit/15bbfac9c7cb42ff56698ac5c00daeddbcdb6a0d)
    - By using `as_chunks_mut`, it is possible read and write multiple slice elements through a single volatile operation. This allows the compiler to optimize the code better (compared to reading the elements one by one).

Thanks to [@KernelFreeze](https://github.com/KernelFreeze) for their contribution!

### [`acpi`](https://github.com/rust-osdev/acpi)

The `acpi` repository contains crates for parsing the ACPI tables – data structures that the firmware of modern
computers use to relay information about the hardware to the OS. This month has seen substantial changes to both
the `acpi` and `aml` crates:

- [We made the types that represent raw ACPI tables public](https://github.com/rust-osdev/acpi/pull/86). This allows library users to work directly with tables such as the
  [FADT](https://docs.rs/acpi/2.3.1/acpi/fadt/struct.Fadt.html) if they need to, which is needed to access power
  management features we don't yet expose. <span class="gray">(published as `acpi v2.3.1`)</span>
- [Native functions are now supported by the AML interpreter!](https://github.com/rust-osdev/acpi/pull/88). A
  'native function' is an AML method that is defined in Rust, rather than using AML bytecode. This is useful for
  defining "up-call" methods (where a method is provided by the OS, and called by the firmware), and will hopefully
  by useful in the future for patching methods in broken tables. A native method can easily be created with
  the [`AmlValue::native_method` constructor](https://docs.rs/aml/0.11.0/aml/value/enum.AmlValue.html#method.native_method).
- However, supporting native functions needed a bit of breakage - `AmlValue` no longer implements `PartialEq` or `Eq`. This actually improves correctness,
  as correctly comparing two `AmlValue`s may require type conversions to be done, and so should be done with the
  [`AmlValue::cmp`](https://docs.rs/aml/0.11.0/aml/value/enum.AmlValue.html#method.cmp) method, but does make `aml
  v0.11.0` a breaking change.
- We also used this opportunity to remove the `legacy_mode` parameter to `AmlContext::new`, which will affect all
  crate users, but makes user's lives simpler - handling old tables will now be done automatically.
- Fallout from `AmlValue` no longer being `Eq` also led to some cleanups in how control flow is handled in the
  interpreter. This is not likely to affect users, but the "fake" error `AmlError::Return` has been removed, which
  is also technically a breaking change in `aml v0.11.0`.
- The AML interpreter now correctly supplies the `\_OS`, `\_OSI`, and `\_REV` objects. These objects were designed
  to allow firmware to detect OS support for new AML features, but [come with a bit of baggage](https://www.kernel.org/doc/html/latest/firmware-guide/acpi/osi.html).
  This is important for supporting running on real hardware, which often assumes the existence of these objects.
  <span class="gray">(published as `aml v0.11.0`)</span>

## Call for Participation
Want to contribute to a Rust OSDev project, but don't know where to start? Pick up one of these outstanding
issues in one of our projects and get started!

If you maintain a Rust OSDev project and are looking for contributors, especially for tasks suited to people
getting started in this space, please [create a PR](https://github.com/rust-osdev/homepage/pulls) against the
`next` branch with the tasks you want to include in the next issue.

- [(`acpi`) Record attempted conversion in `AmlError::IncompatibleValueConversion`](https://github.com/rust-osdev/acpi/issues/53)
- [(`acpi`) Support the new MADT Multiprocessor Wakeup structure](https://github.com/rust-osdev/acpi/issues/87)

## Personal Projects

In this section, we describe updates to personal projects that are not directly related to the `rust-osdev` organization. Feel free to [create a pull request](https://github.com/rust-osdev/homepage/pulls) with the updates of your OS project for the next post.

### [`phil-opp/blog_os`](https://github.com/phil-opp/blog_os)

<span class="gray">(Section written by [@phil-opp](https://github.com/phil-opp))</span>

The [_"Writing an OS in Rust"_](https://os.phil-opp.com) blog received the following updates this month:

- [Translate post-05 to Japanese](https://github.com/phil-opp/blog_os/pull/941)
- [Fix rendering of Japanese translation: Add spaces around some "two asterisk" notations](https://github.com/phil-opp/blog_os/pull/943)
- [Convert `before_build.py` to python3](https://github.com/phil-opp/blog_os/commit/f87cc129fc660473f2d14e9c8d1f8f1e484e105d)
- [Lots of grammar and typo fixes](https://github.com/phil-opp/blog_os/pulls?q=is%3Apr+is%3Aclosed+merged%3A2021-03-22..2021-03-31+)

Thanks to [@woodyZootopia](https://github.com/woodyZootopia), [@alexxroche](https://github.com/alexxroche), and [@ClementNerma](https://github.com/ClementNerma) for their contributions!

The third edition is making progress too. I mostly worked on the post about UEFI booting this month:

- [Describe how to include the uefi crate](https://github.com/phil-opp/blog_os/commit/8740b619a5debe3fa1069c47c61ceed471a3b2f6)
- [Describe how to use various UEFI protocols with the `uefi` crate](https://github.com/phil-opp/blog_os/commit/9c1babd0273ff3d4f632b6e1acf288267138b90f)
- [Provide a high-level explanation on how to create bootloader](https://github.com/phil-opp/blog_os/commit/db47b2702446c1a469e8e064fb090370040bfa2e)

### [`rust-embedded/rust-raspberrypi-OS-tutorials`](https://github.com/rust-embedded/rust-raspberrypi-OS-tutorials)

<span class="gray">(Section written by [@andre-richter](https://github.com/andre-richter))</span>

The [Operating System development tutorials in Rust on the Raspberry Pi](https://github.com/rust-embedded/rust-raspberrypi-OS-tutorials) project
got two more tutorials this month:

- [Tutorial 15 - `Virtual Memory Part 3: Precomputed Translation Tables`](https://github.com/rust-embedded/rust-raspberrypi-OS-tutorials/tree/master/15_virtual_mem_part3_precomputed_tables)
- [Tutorial 16 - `Virtual Memory Part 4: Higher-Half Kernel`](https://github.com/rust-embedded/rust-raspberrypi-OS-tutorials/tree/master/16_virtual_mem_part4_higher_half_kernel)

The two tutorials finally conclude the challenging but rewarding journey of enabling the kernel to execute **from the top of the 64 bit virtual address space**.

Here is a sneak peek of the end result when booting the `kernel` on a **Raspberry Pi 4** (slightly modified to fit on the page):

```
[5.011] Booting on: Raspberry Pi 4
[5.011] MMU online:
[5.011]   --------------------------------------------------------------------------
[5.013]           Virtual            Physical      Size      Attr       Entity
[5.015]   --------------------------------------------------------------------------
[5.017]   0xffff_ffff_8008_0000 --> 0x0008_0000 |  64 KiB | C RO X  | Kernel code
[5.018]   0xffff_ffff_8009_0000 --> 0x0009_0000 | 448 KiB | C RW XN | Kernel data
[5.020]   0xffff_ffff_8011_0000 --> 0x0011_0000 | 512 KiB | C RW XN | Kernel stack
[5.021]   0xffff_ffff_f000_0000 --> 0xfe20_0000 |  64 KiB | D RW XN | BCM GPIO
[5.023]                                                             | BCM PL011 UART
[5.024]   0xffff_ffff_f001_0000 --> 0xff84_0000 |  64 KiB | D RW XN | GICD
[5.026]                                                             | GICC
[5.027]   --------------------------------------------------------------------------
[5.029] Current privilege level: EL1
```

## Join Us?

Are you interested in Rust-based operating system development? Our `rust-osdev` organization is always open to new members and new projects. Just let us know if you want to join! A good way for getting in touch is our [Zulip chat](https://rust-osdev.zulipchat.com).
