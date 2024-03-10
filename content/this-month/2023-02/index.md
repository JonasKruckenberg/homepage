+++
title = "This Month in Rust OSDev: February 2023"
date = 2023-03-08

[extra]
month = "February 2023"
editors = ["phil-opp"]
+++

Welcome to a new issue of _"This Month in Rust OSDev"_. In these posts, we give a regular overview of notable changes in the Rust operating system development ecosystem.

<!-- more -->

This series is openly developed [on GitHub](https://github.com/rust-osdev/homepage/). Feel free to open pull requests there with content you would like to see in the next issue. If you find some issues on this page, please report them by [creating an issue](https://github.com/rust-osdev/homepage/issues/new) or using our <a href="#comment-form">_comment form_</a> at the bottom of this page.

<!--
    This is a draft for the upcoming "This Month in Rust OSDev (February 2023)" post.
    Feel free to create pull requests against the `next` branch to add your
    content here.
    Please take a look at the past posts on https://rust-osdev.com/ to see the
    general structure of these posts.
-->

## Infrastructure and Tooling

In this section, we collect recent updates to `rustc`, `cargo`, and other tooling that are relevant to Rust OS development.

<!--
    Please use the following template:

    ### Summary
    <span class="maintainers">(Section written by [@author](https://github.com/author))</span>

    <text>
-->

### [`rust-lang/rust`](https://github.com/rust-lang/rust)

- [Add `kernel-address` sanitizer support for freestanding targets](https://github.com/rust-lang/rust/pull/99679)
- [Stabilize `#![feature(target_feature_11)]`](https://github.com/rust-lang/rust/pull/99767). Allows enabling target features such as `avx2` for specific functions.
- [Add support for QNX Neutrino to standard library](https://github.com/rust-lang/rust/pull/106673)

### [Cargo: Add '-C' flag for changing current dir before build](https://github.com/rust-lang/cargo/pull/10952)

  This new flag makes `cargo` switch to the specified directory before building.
  This means that the `.cargo/config.toml` file is also read from that directory, which can be very useful when configuring features such as [`build-std`](https://doc.rust-lang.org/nightly/cargo/reference/unstable.html#build-std).


## Announcements, News, and Blog Posts

Here we collect news, blog posts, etc. related to OS development in Rust.

<!--
Please follow this template:

- [Title](https://example.com)
  - (optional) Some additional context
-->

- [Rust now available for Real-Time Operating System and Hypervisor PikeOS](https://www.sysgo.com/press-releases/rust-now-available-for-real-time-operating-system-and-hypervisor-pikeos)
- [Writing an OS in Rust to run on RISC-V](https://gist.github.com/cb372/5f6bf16ca0682541260ae52fc11ea3bb)

## `rust-osdev` Projects

In this section, we give an overview of notable changes to the projects hosted under the [`rust-osdev`] organization.

[`rust-osdev`]: https://github.com/rust-osdev/about

<!--
    Please use the following template:

    ### [`repo_name`](https://github.com/rust-osdev/repo_name)
    <span class="maintainers">Maintained by [@maintainer_1](https://github.com/maintainer_1)</span>

    The `repo_name` crate ...<<short introduction>>...

    We merged the following changes this month:
    <<changelog, either in list or text form>>
-->

### [`uefi-rs`](https://github.com/rust-osdev/uefi-rs)

- [uefi-macros: Fix error tests](https://github.com/rust-osdev/uefi-rs/pull/648)
- [Release `uefi-0.19.1`](https://github.com/rust-osdev/uefi-rs/pull/652)
- [Various fixes for tests under Windows](https://github.com/rust-osdev/uefi-rs/pull/651)
- [uefi: Remove mentions of ruyntime usage from GOP docs](https://github.com/rust-osdev/uefi-rs/pull/613)
- [uefi: Rework `exit_boot_services` API](https://github.com/rust-osdev/uefi-rs/pull/653)
- [Add component name protocols](https://github.com/rust-osdev/uefi-rs/pull/656)
- [uefi: Export `cstr8`, `cstr16`, and entry macros from the root](https://github.com/rust-osdev/uefi-rs/pull/657)
- [Implement `Deref` for `HandleBuffer` and `ProtocolsPerHandle`](https://github.com/rust-osdev/uefi-rs/pull/659)
- [uefi: Improve `Input::read_key` docstring](https://github.com/rust-osdev/uefi-rs/pull/664)
- [Make more tests fail if protocol is missing](https://github.com/rust-osdev/uefi-rs/pull/665)
- [xtask: Fully drop support for build-std](https://github.com/rust-osdev/uefi-rs/pull/585)
- [ci: Simplify the VM jobs](https://github.com/rust-osdev/uefi-rs/pull/668)
- [uefi-macros: Improve entry macro errors](https://github.com/rust-osdev/uefi-rs/pull/670)
- [test-runner: Make `unstable` an optional feature](https://github.com/rust-osdev/uefi-rs/pull/667)
- [xtask: Switch fatfs to latest crates.io release](https://github.com/rust-osdev/uefi-rs/pull/672)

### [`bootloader`](https://github.com/rust-osdev/bootloader)

- [Create kernel stack with correct size and set up a guard page](https://github.com/rust-osdev/bootloader/pull/335)

### [`acpi`](https://github.com/rust-osdev/acpi)

- [Update aml_tester to clap 4](https://github.com/rust-osdev/acpi/pull/149)
- [Add stub implementations for Handler, read_u16 etc.](https://github.com/rust-osdev/acpi/pull/152)
- [Add def_alias, alias shares handle with target](https://github.com/rust-osdev/acpi/pull/153)
- [Update syntax of literal zero](https://github.com/rust-osdev/acpi/pull/148)

Thanks to [@rw-vanc](https://github.com/rw-vanc) for their contributions!

### [`spinning_top`](https://github.com/rust-osdev/spinning_top)

- [Upgrade lock_api to 0.4.7](https://github.com/rust-osdev/spinning_top/pull/13) <span class="gray">(published as `v0.2.5`)</span>

Thanks to [@jannic](https://github.com/jannic) for this contribution!

### [`linked-list-allocator`](https://github.com/rust-osdev/linked-list-allocator)

- [Remove features `const_mut_refs` and `use_spin_nightly`](https://github.com/rust-osdev/linked-list-allocator/pull/77)

Thanks to [@jannic](https://github.com/jannic) for this contribution!


## Other Projects

In this section, we describe updates to Rust OS projects that are not directly related to the `rust-osdev` organization. Feel free to [create a pull request](https://github.com/rust-osdev/homepage/pulls) with the updates of your OS project for the next post.

<!--
    Please use the following template:

    ### [`owner_name/repo_name`](https://github.com/rust-osdev/owner_name/repo_name)
    <span class="maintainers">(Section written by [@your_github_name](https://github.com/your_github_name))</span>

    ...<<your project updates>>...
-->

### [`phil-opp/blog_os`](https://github.com/phil-opp/blog_os)
<span class="maintainers">(Section written by [@phil-opp](https://github.com/phil-opp))</span>

We merged the following changes to the [_Writing an OS in Rust_](https://os.phil-opp.com/) blog this month:

- [[Translation][Chinese] post-09 (edition-2)](https://github.com/phil-opp/blog_os/pull/1189)
  - Thanks to [@weijiew](https://github.com/weijiew) for creating this translation and thanks to [@liuyuran](https://github.com/liuyuran) for reviewing it!
- [Fix typo in edition-3/post-02](https://github.com/phil-opp/blog_os/pull/1193) (thanks to [@emanuele-em](https://github.com/emanuele-em))

## Join Us?

Are you interested in Rust-based operating system development? Our `rust-osdev` organization is always open to new members and new projects. Just let us know if you want to join! A good way for getting in touch is our [Zulip chat](https://rust-osdev.zulipchat.com).
