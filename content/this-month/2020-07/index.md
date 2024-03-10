+++
title = "This Month in Rust OSDev (July 2020)"
date = 2020-08-02

[extra]
month = "July 2020"
authors = [
    "phil-opp",
    "IsaacWoods",
    "GabrielMajeri",
    # add yourself here
]
+++

Welcome to a new issue of _"This Month in Rust OSDev"_. In these posts, we give a regular overview of notable changes in the Rust operating system development ecosystem.

<!-- more -->

This series is openly developed [on GitHub](https://github.com/rust-osdev/homepage/). Feel free to open pull requests there with content you would like to see in the next issue. If you find some issues on this page, please report them by [creating an issue](https://github.com/rust-osdev/homepage/issues/new).

<!--
    This is a draft for the upcoming "This Month in Rust OSDev (July 2020)" post.
    Feel free to create pull requests against the `next` branch to add your
    content here.

    Please take a look at the past posts on https://rust-osdev.com/ to see the
    general structure of these posts.
-->

## Project Updates

In this section, we give an overview of notable changes to the projects hosted under the [`rust-osdev`] organization.

[`rust-osdev`]: https://github.com/rust-osdev/about

### [`bootimage`](https://github.com/rust-osdev/bootimage)

The `bootimage` tool allows the creation of bootable disk images for `bootloader`-based kernels. It also provides a runner executable for `cargo` to make `cargo run` and `cargo test` work using QEMU. In July, the crate was updated to work with cargo's own `build-std` feature instead of relying on the `cargo-xbuild` crate:

- [Add support for building bootloaders using `-Zbuild-std`](https://github.com/rust-osdev/bootimage/pull/62) <span class="gray">(published as `v0.8.1`)</span>
- [Make `cargo bootimage` use `cargo build` instead of `cargo xbuild`](https://github.com/rust-osdev/bootimage/pull/63) <span class="gray">(published as `v0.9.0`)</span>

### [`bootloader`](https://github.com/rust-osdev/bootloader)

The [`bootloader` crate](https://github.com/rust-osdev/bootloader) implements a custom Rust-based bootloader for easy loading of 64-bit ELF executables. In July, we switched the crate from `cargo-xbuild` to the `build-std` feature of cargo and fixed a bug that prevented booting in VirtualBox:

- [Change 1st stage int 13h addressing](https://github.com/rust-osdev/bootloader/pull/123) <span class="gray">(published as `v0.9.6`)</span>
- [Make bootloader buildable with `-Zbuild-std`](https://github.com/rust-osdev/bootloader/pull/125) <span class="gray">(published as `v0.9.7`)</span>
- [Enable rlibc dependency only with `binary` feature](https://github.com/rust-osdev/bootloader/pull/126) <span class="gray">(published as `v0.9.8`)</span>

Thanks to [@rsribeiro](https://github.com/rsribeiro) for their contribution!

We also made some progress on adding UEFI support to the bootloader. Our prototype is now able to set up a pixel-based framebuffer, map a given kernel ELF file into virtual memory, and then pass control to its entry point. The next steps are the construction of the boot information structure, including a memory map. You can find a link to the code and the build instructions in [this comment](https://github.com/phil-opp/blog_os/issues/349#issuecomment-663562464).

### [`acpi`](https://github.com/rust-osdev/acpi)

The `acpi` repository contains crates for parsing the ACPI tables – data structures that the firmware of modern computers use to relay information about the hardware to the OS. This month saw some substantial improvements to our AML handling:

- Objects defined by AML tables exist within a namespace, with objects referred to by paths such as `\_SB.PCI0.ISA.COM1`. Until now, we represented this namespace using a `BTreeMap` between the path (allocated on the heap) and a handle to the
object, meaning that there was both a heap-allocated container, and a heap allocation for every single path within the namespace. [In this PR](https://github.com/rust-osdev/acpi/pull/72), we moved the library to use a new representation that has
a `BTreeMap` for each *level* of the namespace. Because each level of the path can only be 4 characters long (`_SB`, `PCI0`, `ISA`, and `COM1` are the *name segments* in the example above), storing the path of each level no longer requires a heap
allocation per object, which reduces the heap-burden of the library significantly.
- Some more opcodes were implemented: `Target`, `DefShiftLeft`, `DefShiftRight`, `DefLOr`, and `DefAnd`. These all appear in QEMU's tables.
- [A fairly large PR](https://github.com/rust-osdev/acpi/pull/73) was merged that provides the ability to traverse the namespace and initialize devices - this is a mechanism that ACPI provides that allows an OS to initialize hardware that it does
not have drivers for, and is a compulsory step in getting modern chipsets to function properly. This required us to build up a lot of functionality, including namespace traversal, reading and writing from *operation regions*, recursively invoking
control methods, and asking the OS to perform hardware configuration for us (such as reading and writing to IO ports and PCI configuration space). There is still a lot of work in getting all of this working robustly, but this is a great start.

### [`uefi-rs`](https://github.com/rust-osdev/uefi-rs)

The `uefi-rs` crate provides safe and performant wrappers for [UEFI](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface), the successor to the BIOS.

The major changes which happened in this month are:
- We've tested out support for AArch64. No updates to the code are needed, just [add a custom target file](https://github.com/rust-osdev/uefi-rs/blob/e2748687bdafcc21f35e6d4db27b4b1b31bdcf6e/uefi-test-runner/aarch64-unknown-uefi.json) for 64-bit ARM UEFI and then compile your project with it.
- The `LoadedImage` protocol now [exposes the handle](https://docs.rs/uefi/0.4.7/uefi/proto/loaded_image/struct.LoadedImage.html#method.device) for the device where the binary is stored.
- Our CI is now green again! There is [only one test](https://github.com/rust-osdev/uefi-rs/issues/103#issuecomment-604728460) which breaks QEMU, and it's going to stay disabled until it gets fixed.
- Building the repository no longer requires [cargo-xbuild](https://github.com/rust-osdev/cargo-xbuild), we've switched to nightly Cargo's [build-std](https://doc.rust-lang.org/nightly/cargo/reference/unstable.html#build-std) feature.
- Updated documentation in various places.

### [`spinning_top`](https://github.com/rust-osdev/spinning_top)

The `spinning_top` crate provides a simple spinlock implementation based on the abstractions of the [`lock_api`](https://docs.rs/lock_api/0.4.1/lock_api/) crate. We created the crate to provide an alternative to the no-longer maintained [`spin`](https://github.com/mvdnes/spin-rs) crate. In July, the crate received the following updates:

- [Implement `try_lock_weak` for use in `lock` loop](https://github.com/rust-osdev/spinning_top/pull/4) <span class="gray">(published as `v0.1.1`)</span>
- [Upgrade `lock_api` to 0.4.0](https://github.com/rust-osdev/spinning_top/pull/3) <span class="gray">(published as `v0.2.0`)</span>
- [Implement `const_spinlock` convenience function](https://github.com/rust-osdev/spinning_top/pull/5) <span class="gray">(published as `v0.2.1`)</span>

Thanks to [@akiekintveld](https://github.com/akiekintveld) for their contributions!

### [`volatile`](https://github.com/rust-osdev/volatile)

The `volatile` crate provides safe wrapper types for implementing volatile read and write operations. The crate received the following changes this month:

- [Derive Default for Volatile, WriteOnly and ReadOnly](https://github.com/rust-osdev/volatile/pull/10) <span class="gray">(published as `v0.2.7`)</span>
- [Remove `Debug` and `Clone` derives for `WriteOnly`](https://github.com/rust-osdev/volatile/pull/12) <span class="gray">(published as `v0.3.0`)</span>

Thanks to [@Freax13](https://github.com/Freax13) for their contributions!

We are also considering a complete rewrite of the crate to wrap references instead of values directly. The main advantage of this is that it makes working with slices possible. See the [pull request](https://github.com/rust-osdev/volatile/pull/13) for details.

### [`multiboot2`](https://github.com/rust-osdev/multiboot2-elf64)

The `multiboot2` crate provides abstraction types for the boot information of multiboot2 bootloaders. In July, we merged a change that makes the checksum field of the RSDP tag more useful:

- [Add a `validate_checksum` method to rsdp](https://github.com/rust-osdev/multiboot2-elf64/pull/64) <span class="gray">(published as `v0.9.0`)</span>

Thanks to [@dlrobertson](https://github.com/dlrobertson) for this contribution!

### [`cargo-xbuild`](https://github.com/rust-osdev/cargo-xbuild)

The `cargo-xbuild` project provides `cargo` command wrappers to cross-compile the sysroot crates `core` and `alloc`. While there were no updates to the crate itself in July, we added a guide on how to switch from `cargo-xbuild` to cargo's own `build-std` feature to the Readme. You can read it [here](https://github.com/rust-osdev/cargo-xbuild#alternative-the-build-std-feature-of-cargo).

## Personal Projects

In this section, we describe updates to personal projects that are not directly related to the `rust-osdev` organization. Feel free to [create a pull request](https://github.com/rust-osdev/homepage/pulls) with the updates of your OS project for the next post.

### [`IsaacWoods/pebble`](https://github.com/IsaacWoods/pebble)

<span class="gray">(Section written by [@IsaacWoods](https://github.com/IsaacWoods))</span>

Between my work on `acpi` and university commitments, I have not had much time to work on Pebble this month, but
some small clean-ups were made:
- The migration from Travis CI to Github Actions was completed, allowing Pebble to be emulated on CI.
- Pebble uses a HAL (hardware abstraction layer) to separate hardware access from kernel logic. Various parts of the `x86_64` implementation of the HAL were made common to support code-reuse across architectures.
- Some [tests](https://github.com/IsaacWoods/pebble/blob/master/kernel/src/memory/buddy_allocator.rs#L202) were added to the buddy allocator (Pebble's physical memory manager). This was to rule out physical memory
allocation as the cause of a bug where user stacks are occasionally corrupted at the userspace-kernel boundary, which unfortunately has still not been fixed.
- Our [algorithm](https://github.com/IsaacWoods/pebble/blob/master/kernel/hal_x86_64/src/paging.rs#L376-L481) for efficiently mapping arbitrary areas was rewritten and extended to support 1GiB pages.
- Work has started on a *topology layer* - data structures that represent features of the platform such as processors, caches, [NUMA](https://en.wikipedia.org/wiki/Non-uniform_memory_access) nodes, and
microarchitectural information. This will help in allowing Pebble to make more intelligent decisions about scheduling and resource usage in the future.

### [`phil-opp/blog_os`](https://github.com/phil-opp/blog_os)

<span class="gray">(Section written by [@phil-opp](https://github.com/phil-opp))</span>

The main change this month was the migration from `cargo-xbuild` to the `build-std` feature of cargo. This means that we can now use the normal `cargo build`/`cargo run`/`cargo test` commands for our kernel 🎉.

The full list of notable changes is:

- [Migrate code from cargo-xbuild to `-Zbuild-std`](https://github.com/phil-opp/blog_os/pull/835)
    - [Update blog to use `build-std` feature instead of cargo-xbuild](https://github.com/phil-opp/blog_os/pull/836)
- [Link 'This Month in Rust OSDev' posts in status updates section](https://github.com/phil-opp/blog_os/pull/838)

There were also [some contributions](https://github.com/phil-opp/blog_os/pulls?q=is%3Apr+is%3Aclosed+merged%3A2020-06-01..2020-07-01) this month that fixed typos and updated links. Thanks a lot to all contributors!

For the next weeks/months, my plan is to finish the UEFI bootloader implementation and then adjust the blog to work with both the BIOS and UEFI bootloaders. Among other things, this will require that we switch from the VGA text buffer to a pixel-based framebuffer because the VGA text mode is not available with UEFI. While this makes things more complicated (we need to do some simple font rendering and add slice support to `volatile` crate), it will allow us to create our own graphical user interface, render a mouse pointer, or display images.

## Join Us?

Are you interested in Rust-based operating system development? Our `rust-osdev` organization is always open to new members and new projects. Just let us know if you want to join! A good way for getting in touch is our [Zulip chat](https://rust-osdev.zulipchat.com).
