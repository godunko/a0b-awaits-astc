# A0B: Await Mechanism for Ada Tasking

[![Alire](https://img.shields.io/endpoint?url=https://alire.ada.dev/endpoint/a0b_awaits_astc.json)](https://alire.ada.dev/crates/a0b_awaits_astc)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE.txt)

**A0B** (Ada on Board) is a collection of low-level Ada packages designed for embedded development.

The `a0b-awaits-astc` package provides an implementation of the await mechanism (`A0B.Awaits`) designed for environments utilizing **Ada Tasking** (`tasking`/`embedded`/native runtimes).
It allows asynchronous operations (such as callback-driven hardware/driver transactions) to block the calling Ada task efficiently using standard tasking synchronization primitives, leaving other Ada tasks free to execute.

---

## Key Features

* **Tasking Integration:** Suspends the calling Ada task while waiting for an asynchronous event or callback to fire, rather than spinning or putting the entire CPU core to sleep unconditionally.
* **Multi-Task Friendly:** Works naturally in multi-tasking applications built on Ada runtimes that support tasking (such as `embedded-tasking` or standard Ada runtimes).
* **A0B Ecosystem Integration:** Compatible with `A0B.Callbacks` and A0B driver libraries, providing a unified synchronous API for asynchronous drivers across tasking configurations.

---

## Usage

The package provides the same `A0B.Awaits` interface as other `a0b-awaits-*` implementations, allowing hardware drivers or application logic to remain portable regardless of the underlying synchronization mechanism.
For example, below code show how to wait until completion of the asynchronous `Write`:

```ada
with A0B.Awaits;

procedure Write
  (Device     : in out Writable_Device;
   Descriptor : aliased in out A0B.Asynchronous_Operations.Transfer_Descriptor;
   Success    : in out Boolean)
is
   Await : A0B.Awaits.Await;

begin
   Write (Device, Descriptor, A0B.Awaits.Create_Callback (Await), Success);
   A0B.Awaits.Suspend_Until_Callback (Await);
end Write;
```

---

## Requirements

* **Compiler:** GNAT Ada compiler supporting Ada Tasking.
* **Runtime:** An Ada runtime with Tasking support enabled.
* **Dependencies:**
  * `a0b_callbacks`

---

## Installation via Alire

This package is part of the [Alire](https://alire.ada.dev/) ecosystem. You can add it to your project by running:

```
alr with a0b_awaits_astc
```

Or by adding the dependency directly into your project's `alire.toml`:

```toml
[dependencies]
a0b_awaits_astc = "*"
```

---

## License

This project is licensed under the Apache License 2.0. See the `LICENSE.txt` file for full details.
