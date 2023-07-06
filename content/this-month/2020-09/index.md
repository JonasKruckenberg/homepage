+++
title = "This Month in Rust OSDev (September 2020)"
date = 2020-10-04

[extra]
month = "September 2020"
authors = [
    "phil-opp",
    "IsaacWoods",
    # add yourself here
]
+++

Welcome to a new issue of _"This Month in Rust OSDev"_. In these posts, we will give a regular overview of notable changes in the Rust operating system development ecosystem.

<!-- more -->

This series is openly developed [on GitHub](https://github.com/rust-osdev/homepage/). Feel free to open pull requests there with content you would like to see in the next issue. If you find some issues on this page, please report them by [creating an issue](https://github.com/rust-osdev/homepage/issues/new).

<!--
    This is a draft for the upcoming "This Month in Rust OSDev (September 2020)" post.
    Feel free to create pull requests against the `next` branch to add your
    content here.
    Please take a look at the past posts on https://rust-osdev.com/ to see the
    general structure of these posts.
-->

## Project Updates

In this section, we give an overview of notable changes to the projects hosted under the [`rust-osdev`] organization.

[`rust-osdev`]: https://github.com/rust-osdev/about

### [`acpi`](https://github.com/rust-osdev/acpi)

The `acpi` repository contains crates for parsing the ACPI tables – data structures that the firmware of modern computers use to relay information about the hardware to the OS. Lots of work happened this month:

* [Support](https://github.com/rust-osdev/acpi/pull/76) for the `Fixed Memory`, `Word Address Space`, `DWord Address Space`, `QWord Address Space`, `IRQ Format`,
  `DMA Format`, and `I/O Port Descriptor` resource descriptors were added. These appear in `_CRS` objects - objects
  that describe the resources allocated to a particular hardware device. This should be enough support to parse the
  `_CRS` objects of all devices supported by QEMU. Thanks to [`@michaelmelanson`](https://github.com/michaelmelanson) for his contribution!
* [Version `2.0.0` of the `acpi` crate was published](https://github.com/rust-osdev/acpi/pull/75), after [consultation with contributors to the Redox project](https://github.com/rust-osdev/acpi/issues/74).
  This splits the library into two parts - discovering ACPI tables using various methods, and separately, parsing them to discover information about the system.
  This provides much more flexibility in how tables are parsed, allows support for OS-specific tables, and is
  important to the Redox project as it allows parsing of the ACPI tables in userspace. We also used this
  opportunity for breaking changes to clean up a few parts of the library, especially in making our method of
  mapping physical memory ranges safer.
* A new crate, [`rsdp`](https://crates.io/crates/rsdp), was split out from `acpi`. This new crate provides methods
  for searching for the first ACPI table (the Root System Description Pointer (RSDP)) on BIOS systems without
  `alloc` support. This makes it suitable for use from bootloaders and similar applications where heap allocation
  is not supported. All types are reexported by the `acpi` crate, so users can access the same functionality from
  the main library.

### [`x86_64`](https://github.com/rust-osdev/x86_64)

The `x86_64` crate provides various abstractions for `x86_64` systems, including wrappers for CPU instructions, access to processor-specific registers, and abstraction types for architecture-specific structures such as page tables and descriptor tables.

The crate received the following updates in September:

- [Add a function for the `nop` instruction](https://github.com/rust-osdev/x86_64/pull/174) <span class="gray">(published as `v0.11.4`)</span>
- [Don't rely on promotion of `PageTableEntry::new` inside a `const fn`](https://github.com/rust-osdev/x86_64/pull/175) <span class="gray">(published as `v0.11.5`)</span>
    - Thanks a lot to [@ecstatic-morse](https://github.com/ecstatic-morse) and the Rust compiler team for preventing an upcoming breakage in our crate!
- [Add `VirtAddr::is_null`](https://github.com/rust-osdev/x86_64/pull/180) <span class="gray">(published as `v0.11.8`)</span>
- [Decouple instructions into a separate feature flag](https://github.com/rust-osdev/x86_64/pull/179) <span class="gray">(published as `v0.12.0`)</span>
    - See the [corresponding changelog entry](https://github.com/rust-osdev/x86_64/blob/master/Changelog.md#0120--2020-09-23) for a summary of the breaking changes introduced by this pull request.
- [Fix build error on latest nightly](https://github.com/rust-osdev/x86_64/pull/182) caused by new `const_mut_refs` feature gate <span class="gray">(published as `v0.12.1`)</span>
- [Add remaining GDT flags](https://github.com/rust-osdev/x86_64/pull/181), which also makes our GDT descriptors compatible with the `syscall`/`sysenter` instructions
- [Fix another build error on latest nightly](https://github.com/rust-osdev/x86_64/pull/186), this time caused by the new `const_fn_fn_ptr_basics` feature gate <span class="gray">(published together with [#181](https://github.com/rust-osdev/x86_64/pull/181) as `v0.12.2`)</span>

Thanks to [@ecstatic-morse](https://github.com/ecstatic-morse), [@toku-sa-n](https://github.com/toku-sa-n), [@h33p](https://github.com/h33p), and [@josephlr](https://github.com/josephlr) for their contributions!

We would also like to welcome [@josephlr](https://github.com/josephlr) to our `x86_64` review and maintenance team!

### [`volatile`](https://github.com/rust-osdev/volatile)

The `volatile` crate provides safe wrapper types for implementing volatile read and write operations. This month, we published version `0.4.0`, which [completely rewrites the crate based on reference values](https://github.com/rust-osdev/volatile/pull/13). Instead of wrapping the target value directly as e.g. `Volatile<u32>`, the new implementation works by wrapping reference types such as `Volatile<&mut u32>`. See [our completely revamped documentation](https://docs.rs/volatile/0.4.6/volatile/struct.Volatile.html) for more details.

The main advantage of the new reference-based implementation is that it is now possible to work with volatile arrays and slices, at least on nightly Rust. Through the [`index`](https://docs.rs/volatile/0.4.6/volatile/struct.Volatile.html#method.index) and [`index_mut`](https://docs.rs/volatile/0.4.6/volatile/struct.Volatile.html#method.index_mut) methods it is possible to create sub-slices, which can then be read and written efficiently through the [`copy_into_slice`](https://docs.rs/volatile/0.4.6/volatile/struct.Volatile.html#method.copy_into_slice) and [`copy_from_slice`](https://docs.rs/volatile/0.4.6/volatile/struct.Volatile.html#method.copy_from_slice) methods. All these operations perform volatile memory accesses, so that the compiler won't optimize them away.

### [`pci_types`](https://github.com/rust-osdev/pci_types)

`pci_types` is a new library in the Rust OSDev organisation that provides types for accessing and configuring PCI
devices from Rust operating systems. Lots of this code (e.g. identifying devices by class codes) can be shared
between projects, and would benefit from community contributions. This month, work started on some types for
representing PCIe addresses, the layout of the configuration space for *endpoints*, device types, and a trait that
allows the library to access the PCIe configuration space in whichever way the platform exposes it.

Thanks to [@toku-sa-n](https://github.com/toku-sa-n) for their [contribution](https://github.com/rust-osdev/pci_types/pull/1)!

### [`uefi-rs`](https://github.com/rust-osdev/uefi-rs)

The `uefi-rs` crate provides safe and performant wrappers for [UEFI](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface), the successor to the BIOS. In September, the crate was [updated to Rust's new inline assembly](https://github.com/rust-osdev/uefi-rs/pull/167) implemenation. We also published version `0.6.0` of the crate, including all the improvements added in the past two months.

Thanks to [@toku-sa-n](https://github.com/toku-sa-n) for their contribution!

### [`bootloader`](https://github.com/rust-osdev/bootloader)

The `bootloader` crate implements a custom Rust-based bootloader for easy loading of 64-bit ELF executables. This month, we published versions `0.9.9` to `0.9.11` to fix build errors on the latest nightlies, caused by the new feature gate names for some `const fn` features.

We also made some more progress on the rewrite with UEFI support. It now [passes additional framebuffer information](https://github.com/rust-osdev/bootloader/commit/3237429457d3df58080fa284d9c2b7138e3fff3a) and the [address of the RSDP structure of ACPI](https://github.com/rust-osdev/bootloader/commit/694d3139fe24bce0184da3bb9096e6540ee1fa3d) in the boot info. (We later updated the RSDP code to [use the new `rsdp` crate](https://github.com/rust-osdev/bootloader/commit/cb8345bd3c45fc3c56a631a3b416137c45f828f9)). The bootloader also gained support for [setting up a recursive page table mapping](https://github.com/rust-osdev/bootloader/commit/34a5da852ba8f6b0abfafb5c5a68adc4cae638fa), which makes it almost feature-equivalent with the current implementation. There are still a few things missings, but it should be ready to be published soon.

### [`cargo-xbuild`](https://github.com/rust-osdev/cargo-xbuild)

The `cargo-xbuild` project provides `cargo` command wrappers to cross-compile the sysroot crates `core` and `alloc`. This month, we merged some maintenance updates to increase platform compatibility:

- [Remove fs2 dependency for broader platform support](https://github.com/rust-osdev/cargo-xbuild/pull/91) <span class="gray">(published as `v0.6.1`)</span>
- [Fix winapi issues from flock() rework](https://github.com/rust-osdev/cargo-xbuild/pull/94) <span class="gray">(published as `v0.6.2`)</span>

Thanks to [@pfmooney](https://github.com/pfmooney) for these contributions!

Even though we still maintain the `cargo-xbuild` crate, we recommend switching to cargo's own `build-std` feature that is always up-to-date with the latest Rust/Cargo changes. We wrote a short guide on how to switch to it, which is available [in our Readme](https://github.com/rust-osdev/cargo-xbuild#alternative-the-build-std-feature-of-cargo).

### [`uart_16550`](https://github.com/rust-osdev/uart_16550)

The `uart_16550` crate provides basic support for serial port I/O for 16550-compatible UARTs. Like the `x86_64` and `bootloader` crates, this crate received some dependency updates this month to fix nightly breakage. In the process, we released versions `0.2.8` to `0.2.10`. Since none of these versions are semver-breaking, a normal `cargo update` should suffice to update to the latest version.

## Personal Projects

In this section, we describe updates to personal projects that are not directly related to the `rust-osdev` organization. Feel free to [create a pull request](https://github.com/rust-osdev/homepage/pulls) with the updates of your OS project for the next post.

### [`IsaacWoods/pebble`](https://github.com/IsaacWoods/pebble)

<span class="gray">(Section written by [@IsaacWoods](https://github.com/IsaacWoods))</span>

A fairly large amount of progress has been made on Pebble over the last two months, and a prototype of Pebble's
defining feature (easy message passing between tasks) is now working!

* The first, basic, version of Pebble's wire format, [ptah](https://github.com/IsaacWoods/pebble/tree/master/lib/ptah) has been completed.
  This first version is implemented as a Serde format, but libraries could be written for any language that can
  manipulate a byte stream.
* Two system calls, `send_message` and `get_message`, were added that allows a message (a series of bytes, and
  optionally a number of *handles* (effectively, permission to access a certain *kernel object*)) to be sent
  down a *channel*, between two tasks.
* Infrastructure for another of Pebble's key features, *services*, was added. This allows userspace tasks to
  advertise their ability to provide some kind of 'service' to other tasks. A task called [`echo`](https://github.com/IsaacWoods/pebble/blob/master/user/echo/src/main.rs)
  was built to test this - it provides a service that simply echos any messages sent down it back to the sending
  task.
* Work started on the Platform Bus, a concept inspired by another hobby OS [managarm's `mbus`](https://github.com/managarm/managarm/blob/master/docs/src/design/mbus/index.md).
  All hardware devices on the platform will be added to the Platform Bus by *Bus Drivers*, and described using *properties* (as an
  example, a PCI device will have properties such as `pci.vendor_id`, and `pci.class`). *Device drivers* will be
  able to apply to manage devices by sending a *Filter* to the Platform Bus, which specifies which devices they are
  able to handle, based on a device's properties. In the future, Platform Bus will be responsible for handling all
  PCI, USB, and hardwired devices on all platforms.
* The first *Bus Driver* was added to manage PCI devices. It uses a new system call, `pci_get_info`, to get the raw
  information about PCI from the kernel, and then creates a Platform Bus device for each function, with the correct
  properties. It uses the new `pci_types` library in the Rust OSDev organisation to identify each device (in the
  future, this will be extended to know about specific vendor+device ID combinations, to identify specific devices
  such as a particular graphics card).

### [`phil-opp/blog_os`](https://github.com/phil-opp/blog_os)

<span class="gray">(Section written by [@phil-opp](https://github.com/phil-opp))</span>

This month, the "Writing an OS in Rust" blog received a few minor updates:

- [Update Zola to 0.11.0](https://github.com/phil-opp/blog_os/pull/850)
- [Update `x86_64` to v0.12.1](https://github.com/phil-opp/blog_os/pull/858)
- [Use new `const_mut_refs` feature gate](https://github.com/phil-opp/blog_os/pull/860)
- [Update to zola v0.12.1](https://github.com/phil-opp/blog_os/pull/861)

Apart from that, I did a lot of preparation for the upcoming switch to the UEFI bootloader. My prototype implementation of the `blog_os` system on top of the new UEFI bootloader has now reached feature parity with the existing implementation:

![QEMU output of new bootloader implementation, including local APIC and I/O APIC debug output, the timer interrupt dots, and the keyboard input "Hello!!!"](blog_os-rewrite.png)

The output looks different now because we are using a pixel based framebuffer instead of the VGA text mode. This is required because the VGA text mode is no longer supported with UEFI. For the framebuffer implementation I'm using the [new version of the `volatile` crate](#volatile) and the [`font8x8`](https://docs.rs/font8x8/0.2.5/font8x8/) crate for font rendering.

You can also see some log output related to the APIC interrupt controller (not to be confused with the ACPI standard). I'm using the APIC instead of the legacy PIC because the latter is not supported anymore on most UEFI systems. For that, I started working on a new `apic` crate, which will include abstractions for the registers of the local APIC and the IOAPIC.

For the coming month(s), I'm planning to revamp the "Writing an OS in Rust" blog based on this prototype implementation. This will require complete rewrites of the [_VGA Text Mode_](https://os.phil-opp.com/vga-text-mode/) and [_Hardware Interrupts_](https://os.phil-opp.com/hardware-interrupts/) posts, an update of the bootloader build process in [_A Minimal Rust Kernel_](https://os.phil-opp.com/minimal-rust-kernel/), and replacing the QEMU screenshots across all posts. So I expect that it will take some time until the new version is ready.

### [`andre-richter/qemu-exit`](https://github.com/andre-richter/qemu-exit)

<span class="gray">(Section written by [@andre-richter](https://github.com/andre-richter))</span>

Version `1.0.x` of the crate has been released!

`qemu-exit` is a crate that allows you quit a running QEMU session with a user-defined exit code. This is useful for `unit` or `integration tests` of bare-metal software (e.g. OS kernels) that are tested in QEMU.

The crate supports the following architectures:
- `AArch64`
- `RISC-V 64`
- `x86_64`

If you want to see the crate in action, you can have a look at how it is used in the [rust-raspberrypi-OS-tutorials project](https://github.com/rust-embedded/rust-raspberrypi-OS-tutorials/tree/master/13_integrated_testing#quitting-qemu-with-user-defined-exit-codes).

Shoutout to [@phil-opp](https://github.com/phil-opp) for inspiring this crate with his original blog post and to [@Skallwar](https://github.com/Skallwar) for his many contributions.

## Join Us?

Are you interested in Rust-based operating system development? Our `rust-osdev` organization is always open to new members and new projects. Just let us know if you want to join! A good way for getting in touch is our [gitter channel](https://gitter.im/rust-osdev/Lobby).
