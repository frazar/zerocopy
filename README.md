<!-- Copyright 2022 The Fuchsia Authors. All rights reserved.
Use of this source code is governed by a BSD-style license that can be
found in the LICENSE file. -->

# zerocopy

Utilities for safe zero-copy parsing and serialization.

This crate provides utilities which make it easy to perform zero-copy
parsing and serialization by allowing zero-copy conversion to/from byte
slices.

This is enabled by three core marker traits, each of which can be derived
(e.g., `#[derive(FromBytes)]`):
- `FromBytes` indicates that a type may safely be converted from an
  arbitrary byte sequence
- `AsBytes` indicates that a type may safely be converted *to* a byte
  sequence
- `Unaligned` indicates that a type's alignment requirement is 1

Types which implement a subset of these traits can then be converted to/from
byte sequences with little to no runtime overhead.

Note that these traits are ignorant of byte order. For byte order-aware
types, see the `byteorder` module.

## Features

`alloc`: By default, `zerocopy` is `no_std`. When the `alloc` feature is
enabled, the `alloc` crate is added as a dependency, and some
allocation-related functionality is added.

`simd`: When the `simd` feature is enabled, `FromBytes` and `AsBytes` impls
are emitted for all stable SIMD types which exist on the target platform.
Note that the layout of SIMD types is not yet stabilized, so these impls may
be removed in the future if layout changes make them invalid. For more
information, see the Unsafe Code Guidelines Reference page on the [Layout of
packed SIMD vectors][simd-layout].

`simd-nightly`: Enables the `simd` feature and adds support for SIMD types
which are only available on nightly. Since these types are unstable, support
for any type may be removed at any point in the future.

[simd-layout]: https://rust-lang.github.io/unsafe-code-guidelines/layout/packed-simd-vectors.html

## Dislcaimer

Disclaimer: Zerocopy is not an officially supported Google product.
