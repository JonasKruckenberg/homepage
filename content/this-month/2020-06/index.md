+++
title = "This Month in Rust OSDev (June 2020)"
date = 2020-07-12

[extra]
month = "June 2020"
authors = [
    "phil-opp",
    # add yourself here
]
+++

Welcome to a new issue of _"This Month in Rust OSDev"_. In these posts, we will give a regular overview of notable changes in the Rust operating system development ecosystem.

<!-- more -->

This series is openly developed [on GitHub](https://github.com/rust-osdev/homepage/). Feel free to open pull requests there with content you would like to see in the next issue. If you find some issues on this page, please report them by [creating an issue](https://github.com/rust-osdev/homepage/issues/new).

<!--
    This is a draft for the upcoming "This Month in Rust OSDev (June 2020)" post.
    Feel free to create pull requests against the `next` branch to add your
    content here.

    Please take a look at the past posts on https://rust-osdev.com/ to see the
    general structure of these posts.
-->

## Project Updates

In this section, we give an overview of notable changes to the projects hosted under the [`rust-osdev`] organization.

[`rust-osdev`]: https://github.com/rust-osdev/about

### [`x86_64`](https://github.com/rust-osdev/x86_64)

The `x86_64` crate provides various abstractions for `x86_64` systems, including wrappers for CPU instructions, access to processor-specific registers, and abstraction types for architecture-specific structures such as page tables and descriptor tables.

In June, the crate received some smaller improvements:

- [Remove needless `try_into` calls](https://github.com/rust-osdev/x86_64/pull/159) to fix clippy warnings
- [Correct Cr2::read documentation](https://github.com/rust-osdev/x86_64/pull/161)
- [Export `PhysAddrNotValid` and `VirtAddrNotValid`](https://github.com/rust-osdev/x86_64/pull/163) <span class="gray">(published as `v0.11.1`)</span>

Thanks to [@samueltardieu](https://github.com/samueltardieu) and [@leecannon](https://github.com/leecannon) for their contributions!

### [`cargo-xbuild`](https://github.com/rust-osdev/cargo-xbuild)

The `cargo-xbuild` project provides `cargo` command wrappers to cross-compile the sysroot crates `core` and `alloc`. This month, support for the `cargo-features` manifest key was added and a deprecated dependency was replaced:

- [Propagate `cargo-features` from project's Cargo.toml](https://github.com/rust-osdev/cargo-xbuild/pull/82) <span class="gray">(published as `v0.5.34`)</span>
- [Replace tempdir with tempfile](https://github.com/rust-osdev/cargo-xbuild/pull/84) <span class="gray">(published as `v0.5.35`)</span>

Thanks to [@eggyal](https://github.com/eggyal) and [@Eijebong](https://github.com/Eijebong) for these contributions!

### [`bootloader`](https://github.com/rust-osdev/bootloader)

The [`bootloader` crate](https://github.com/rust-osdev/bootloader) implements a custom Rust-based bootloader for easy loading of 64-bit ELF executables. This month, we fixed a newly introduced Rust warning:

- [Rename `_improper_ctypes_check` functions](https://github.com/rust-osdev/bootloader/pull/122) <span class="gray">(published as `v0.9.5`)</span>

Thanks to [@Freax13](https://github.com/Freax13) for this contribution!

While we do not have to report any news yet, we are still working on a rewrite of the crate in order to make it more robust (use Rust instead of assembly for boot stages) and composable (in order to add UEFI and multiboot2 support).

### [`acpi`](https://github.com/rust-osdev/acpi)

The `acpi` repository contains crates for parsing the ACPI tables – data structures that the firmware of modern computers use to relay information about the hardware to the OS. This month, the crate received two small improvements to the [RSDP](https://wiki.osdev.org/RSDP)-related code:

- [Use RSDP's length field in revisions 1+](https://github.com/rust-osdev/acpi/commit/43df4bc79611d311c4a50978ebc4babe78b46074)
- [Use fold to sum RSDP bytes](https://github.com/rust-osdev/acpi/commit/a37cf48429334dc3dfd98e065656c374cc907a4a)

### [`uefi`](https://github.com/rust-osdev/uefi-rs)

The `uefi` crate provides abstractions for the [`UEFI`](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface) standard that replaces the traditional BIOS on modern systems. This month, the contribution docs were extended with information how to add new UEFI protocols:

- [Tutorial on how to add protocols](https://github.com/rust-osdev/uefi-rs/commit/56375412e62d41122aba5b2c86c365373ca31ecd)

## Personal Projects

In this section, we describe updates to personal projects that are not directly related to the `rust-osdev` organization. Feel free to [create a pull request](https://github.com/rust-osdev/homepage/pulls) with the updates of your OS project for the next post.

### [`phil-opp/blog_os`](https://github.com/phil-opp/blog_os)

<span class="gray">(Section written by [@phil-opp](https://github.com/phil-opp))</span>

In June, I pushed two small improvements to the `blog_os` repository and the _Writing an OS in Rust_ blog:

- [Create a testable trait for printing test messages automatically](https://github.com/phil-opp/blog_os/pull/816). This required updates to some blog posts:
    - [Update Testing post to use Testable trait for automatic printing](https://github.com/phil-opp/blog_os/pull/817)
    - Update remaining posts: [Remove superfluous printing from tests](https://github.com/phil-opp/blog_os/pull/819)
- [Do a volatile read in stack_overflow test to avoid tail recursion](https://github.com/phil-opp/blog_os/pull/818). This required an update to the _Double Faults_ post:
    - [Update Double Faults post to prevent tail recursion in test](https://github.com/phil-opp/blog_os/pull/820)

There were also [lots of small contributions](https://github.com/phil-opp/blog_os/pulls?q=is%3Apr+is%3Aclosed+merged%3A2020-06-01..2020-07-01) this month that fixed typos and dead links and updated the Chinese translation. Thanks a lot to all contributors!

## Join Us?

Are you interested in Rust-based operating system development? Our `rust-osdev` organization is always open to new members and new projects. Just let us know if you want to join! A good way for getting in touch is our [Zulip chat](https://rust-osdev.zulipchat.com).
