+++
title = "This Month in Rust OSDev (December 2020)"
date = 2021-01-07

[extra]
month = "December 2020"
authors = [
    "phil-opp",
    "IsaacWoods",
    "GabrielMajeri",
    # add yourself here
]
+++

Welcome to a new issue of _"This Month in Rust OSDev"_. In these posts, we will give a regular overview of notable changes in the Rust operating system development ecosystem.

<!-- more -->

This series is openly developed [on GitHub](https://github.com/rust-osdev/homepage/). Feel free to open pull requests there with content you would like to see in the next issue. If you find some issues on this page, please report them by [creating an issue](https://github.com/rust-osdev/homepage/issues/new) or using our new <a href="#comment-form">_comment form_</a> at the bottom of this page.

<!--
    This is a draft for the upcoming "This Month in Rust OSDev (December 2020)" post.
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

In December, we published the following three new releases:

#### [`v0.12.4`](https://github.com/rust-osdev/x86_64/pull/220)

- [Add and fix some intra-doc links](https://github.com/rust-osdev/x86_64/pull/208)
- [GDT: Add `load_unchecked`, `from_raw_slice`, and `as_raw_slice`](https://github.com/rust-osdev/x86_64/pull/210)
- [Fix bad conversion from llvm_asm! to asm!](https://github.com/rust-osdev/x86_64/pull/218)
    - _Heads up:_ [Rust does not check the assembly in `asm!` unless its used](https://github.com/rust-lang/rust/issues/80440)

#### [`v0.13.0` (breaking)](https://github.com/rust-osdev/x86_64/pull/223)

- [Also return flags for `MapperAllSizes::translate()`](https://github.com/rust-osdev/x86_64/pull/207)
- [Restructure the `TranslateResult` type and create separate `Translate` trait](https://github.com/rust-osdev/x86_64/pull/211)
- [Rename `PhysToVirt` trait to `PageTableFrameMapping`](https://github.com/rust-osdev/x86_64/pull/214))
- [Use custom error types instead of `()`](https://github.com/rust-osdev/x86_64/pull/199))
- [Remove deprecated items](https://github.com/rust-osdev/x86_64/pull/223/commits/2df2b97cb53e593b89ab2dbee6669e63d4898386): `UnusedPhysFrame`, `ExceptionStackFrame`, `VirtAddr::new_unchecked`, `interrupts::enable_interrupts_and_hlt`
- [Make `DescriptorTablePointer::base` a `VirtAddr`](https://github.com/rust-osdev/x86_64/pull/215))
- [Change return type of `read_rip` to `VirtAddr`](https://github.com/rust-osdev/x86_64/pull/216))
- [Make writing the RFLAGS register unsafe](https://github.com/rust-osdev/x86_64/pull/219))
- [Remove `PortReadWrite` trait, which is no longer needed](https://github.com/rust-osdev/x86_64/pull/217))
- [Relaxe `Sized` requirement for `FrameAllocator` in `Mapper::map_to`](https://github.com/rust-osdev/x86_64/pull/204)

#### [`v0.13.1`](https://github.com/rust-osdev/x86_64/commit/4d5058c1a1c3873294b92a628be0bb151d37ca6a)

- [PCID support instructions](https://github.com/rust-osdev/x86_64/pull/169)

Thanks to [@mental32](https://github.com/mental32), [@vinaychandra](https://github.com/vinaychandra), [@tomaka](https://github.com/tomaka), [@haraldh](https://github.com/haraldh), [@tscs37](https://github.com/tscs37), and [@toku-sa-n](https://github.com/toku-sa-n) for their contributions!

### [`volatile`](https://github.com/rust-osdev/volatile)

The `volatile` crate provides a safe wrapper type for implementing volatile read and write operations. This is useful for accessing memory regions that have side-effects, such as memory-mapped hardware registers or framebuffers. In December, we added to new methods for creating read/write-only `Volatile` instances:

- [Add methods to restrict access](https://github.com/rust-osdev/volatile/pull/19) <span class="gray">(published as `v0.4.3`)</span>

### [`bootimage`](https://github.com/rust-osdev/bootimage)

The `bootimage` tool allows the creation of bootable disk images for `bootloader`-based kernels. It also provides a runner executable for `cargo` to make `cargo run` and `cargo test` work using QEMU. This month, we fixed a nightly breakage:

- [Fix nightly breakage of doctests in workspaces](https://github.com/rust-osdev/bootimage/pull/69) <span class="gray">(published as `v0.10.2`)</span>

### [`cargo-xbuild`](https://github.com/rust-osdev/cargo-xbuild)

The `cargo-xbuild` project provides `cargo` command wrappers to cross-compile the sysroot crates `core` and `alloc`. This month, we merged a small error reporting improvement:

- [Don't panic on metadata errors](https://github.com/rust-osdev/cargo-xbuild/pull/100) <span class="gray">(published as `v0.6.4`)</span>

Thanks to [@parasyte](https://github.com/parasyte) for this contribution!

Even though we still maintain the `cargo-xbuild` crate, we recommend switching to cargo's own `build-std` feature that is always up-to-date with the latest Rust/Cargo changes. We wrote a short guide on how to switch to it, which is available [in our Readme](https://github.com/rust-osdev/cargo-xbuild#alternative-the-build-std-feature-of-cargo).

### [`bootloader`](https://github.com/rust-osdev/bootloader)

The `bootloader` crate implements a custom Rust-based bootloader for easy loading of 64-bit ELF executables. In December, we merged the following changes:

- [Document the build process](https://github.com/rust-osdev/bootloader/pull/134)
- [Fix CI after breaking change of Github Actions](https://github.com/rust-osdev/bootloader/compare/530b26f86ae0c7cf8906de0d7f5184bb206bcb7c...d8f7a20bb24e9f1d36f86010e1ce00bdfc51d045)

Thanks to [@Luis-Hebendanz](https://github.com/Luis-Hebendanz) for their contribution!

### [`uefi-rs`](https://github.com/rust-osdev/uefi-rs)

The `uefi` crate provides safe and performant wrappers for [UEFI](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface), the successor to the BIOS. In December, we merged the following changes:

- [Clarify how new protocols can be defined](https://github.com/rust-osdev/uefi-rs/pull/185)
- [Add support for the device path protocol](https://github.com/rust-osdev/uefi-rs/pull/187)
- [Fix breakage with the latest nightlies](https://github.com/rust-osdev/uefi-rs/pull/188)
- [Publish new versions of the crates](https://github.com/rust-osdev/uefi-rs/pull/189)

Thanks to [@avirule](https://github.com/avirule) for their contribution!

### [`acpi`](https://github.com/rust-osdev/acpi)

The `acpi` repository contains crates for parsing the ACPI tables – data structures that the firmware of modern
computers use to relay information about the hardware to the OS. In December, we:

- [Exposed the Power Management Timer, part of the ACPI hardware platform](https://github.com/rust-osdev/acpi/pull/83). Thanks to [@toku-sa-n](https://github.com/toku-sa-n) for their contribution!
- [Improved the behaviour of the BIOS RSDP search algorithm](https://github.com/rust-osdev/acpi/issues/79). If
  you're coming across the same problem as the reporter, try updating to `rsdp v1.1.0` or `acpi v2.2.0`.
- [Improved codegen for the `choice!` macro](https://github.com/rust-osdev/acpi/commit/47c4aec17e7995beeaad004447505ab08b84578a). This fixes limitations in how large parsers can get before they break `rustc`'s type limit.
  The new version allows better ergonomics, but also generates much less work for the type checker and so speeds up the compilation of the `aml` crate.
- Fixed a few bugs in the handling of `DefBuffer` and `DefPackage` objects ([1](https://github.com/rust-osdev/acpi/commit/4286dfc6a9f683dc652cd019bbc6d018e96e8359), [2](https://github.com/rust-osdev/acpi/commit/0c64768a9eb415a0a9081adf0ebec2ff3aa50503), [3](https://github.com/rust-osdev/acpi/commit/6146d0fa2d22a4191f5d13bd653f8d45c1edb796)).

## Personal Projects

In this section, we describe updates to personal projects that are not directly related to the `rust-osdev` organization. Feel free to [create a pull request](https://github.com/rust-osdev/homepage/pulls) with the updates of your OS project for the next post.


### [`phil-opp/blog_os`](https://github.com/phil-opp/blog_os)

<span class="gray">(Section written by [@phil-opp](https://github.com/phil-opp))</span>

We merged the following changes to the [_Writing an OS in Rust_](https://os.phil-opp.com/) blog this month:

- [Translate post-03 to Japanese](https://github.com/phil-opp/blog_os/pull/887)
- [Rename edition folders](https://github.com/phil-opp/blog_os/pull/890)
- [Minor grammar fixes](https://github.com/phil-opp/blog_os/pull/894)
- [Add filepath to code block](https://github.com/phil-opp/blog_os/pull/893)
- [Fix broken QEMU link](https://github.com/phil-opp/blog_os/pull/888)

Thanks to [@woodyZootopia](https://github.com/woodyZootopia), [@JohnTitor](https://github.com/JohnTitor), [@delta1](https://github.com/delta1), and [@16yuki0702](https://github.com/16yuki0702) for their contributions!

#### Third Edition?

Last month, I mentioned that I'm [working on a new revision of the blog](@/this-month/2020-11/index.md#phil-opp-blog-os) with support for UEFI booting, a pixel-based framebuffer, and the APIC interrupt controller. I also mentioned that the Cargo team approved my [proposal to replicate some `cargo/config` settings in the `Cargo.toml`](https://internals.rust-lang.org/t/proposal-move-some-cargo-config-settings-to-cargo-toml/13336), which will be required for the new build system of the blog. In December, we now saw the first pull request based on this proposal: [_Expose build.target .cargo/config setting as packages.target in Cargo.toml_](https://github.com/rust-lang/cargo/pull/9030). Thanks a lot to [@Ekleog](https://github.com/Ekleog) for tackling this!

While I had planned the new revision for a long time already, there was still one open question: Should I integrate the changes into the second edition or start a new, third edition of the blog? Originally, I tended to integrate the changes into the second edition since a new edition is a lot of work and only a few posts would require substantial changes anyway. However, the recent translation efforts by many great people (thanks to all contributors!) have changed my mind on this because I don't want to throw away any of their work. For this reason I decided to start working on a new, _third edition of the blog 🎉!_

Apart from the changes mentioned above, the new edition will have some additional "fun" posts at the start that show how to create a simple shell and some basic games (instead of directly jumping into interrupt handling and memory management). In addition to the [_Testing_](https://os.phil-opp.com/testing/) post we already have, there will be some posts about debugging with GDB and QEMU. There will also be some design improvements such as a refreshed index page and a dark mode. I hope you like these ideas and I look forward to releasing a first draft of the new edition soon!

### [`phil-opp/linked-list-allocator`](https://github.com/phil-opp/linked-list-allocator)

<span class="gray">(Section written by [@phil-opp](https://github.com/phil-opp))</span>

The `linked_list_allocator` crate provides a simple heap allocator that is usable on `no_std` systems. It keeps track of free memory blocks by turning them into a linked list data structure.

In December, the crate received the following updates:

- [Use new nightly Allocator trait](https://github.com/phil-opp/linked-list-allocator/pull/42)
- [Build on stable without features](https://github.com/phil-opp/linked-list-allocator/pull/43) <span class="gray">(published as `v0.8.7`)</span>
- [Fix: do not require alloc crate](https://github.com/phil-opp/linked-list-allocator/pull/44) <span class="gray">(published as `v0.8.8`)</span>
- [Don't require nightly for `use_spin` feature](https://github.com/phil-opp/linked-list-allocator/pull/46) <span class="gray">(published as `v0.8.9`)</span>
- [Make hole module public for external uses](https://github.com/phil-opp/linked-list-allocator/pull/47) <span class="gray">(published as `v0.8.10`)</span>
- [Add new use_spin_nightly feature](https://github.com/phil-opp/linked-list-allocator/pull/49) <span class="gray">(published as `v0.8.11`)</span>

Thanks to [@haraldh](https://github.com/haraldh), [@MarcoCicognani](https://github.com/MarcoCicognani), and [@thalesfragoso](https://github.com/thalesfragoso) for their contributions!.

### [`lucis-fluxum/ps2-rs`](https://github.com/lucis-fluxum/ps2-rs)

<span class="gray">(Section written by [@lucis-fluxum](https://github.com/lucis-fluxum))</span>

I pushed [release v0.1.1](https://docs.rs/ps2/0.1.1/ps2/) this month, which is mainly a documentation update aiming
to improve understanding of how to use the library. I've also added links to some reading material that helped me
understand the PS/2 protocol better and much of the surrounding terminology. May the old keyboards live on!

## Join Us?

Are you interested in Rust-based operating system development? Our `rust-osdev` organization is always open to new members and new projects. Just let us know if you want to join! A good way for getting in touch is our [Zulip chat](https://rust-osdev.zulipchat.com).
