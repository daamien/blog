---
date: 2026-11-18 12:30:36
layout: post
title: "PGRX 0.20: Build your own RPM/DEB packages"
description: ""
category: english
tags: ["PostgreSQL"]
---


Writing a Postgres extension in Rust with [pgrx](https://github.com/pgcentralfoundation/pgrx)
is a lot of fun. You write a few functions in Rust, launch an instance with `cargo pgrx run`
and you can call these functions directly inside Postgres.

With `cargo pgrx test` and `cargo pgrx regress`, you can run unit tests and regression checks
against any major version of Postgres until everything is green. Then comes the last step,
and it's the least fun one: **shipping it**.

Shipping a PostgreSQL means packages. A lot of packages. One for each distro (RPM/DEB), for each
Postgres Major versions and for each CPU architecture. For instance with the
[PostgreSQL Anonymizer] extension, we publish [24 packages] for every release...


[PostgreSQL Anonymizer]: https://postgresql-anonymizer.readthedocs.io/
[24 packages]: https://gitlab.com/dalibo/postgresql_anonymizer/-/releases/3.3.2

Until now, the `cargo pgrx package` command gave you a nice directory tree and
politely wished you good luck with the rest. With pgrx 0.20, that last step is gone. :-)

<!--MORE-->

![Photo Credit Ayrat Yusupov]({{ site.baseurl }}/assets/img/2026_pexels-ayratyusupov-12354555.jpg)

## What's new in pgrx 0.20

But before jumping into that, follow me in a quick overwiew of the new features of
PGRX 0.20 which was released a few days ago !

This is a big release, with some breaking changes. Here are the highlights:

- **Generated C macros:** pgrx now generates automatically Rust versions for all
  the Postgres C macros and static inline functions at build time. The old
  hand-written ports are gone, so some code will need an update.
- **Postgres 15 to 19:** support for Postgres 13 and 14 are dropped, and Postgres 19 is
  now supported officially
- **Safety fixes:** several safe APIs that could lead to undefined behavior have
  been fixed, which changes a few signatures.
- **`cargo pgrx migrate`:** assemble your upgrade scripts from small SQL fragments,
  one file per change.
- **Lighter builds:** extensions and `cargo-pgrx` itself compile fewer crates,
  and installing `cargo-pgrx` on Linux no longer requires the OpenSSL headers.
- **RPM and DEB packages:** the one I want to talk about today!

## Packages, Out of the Box

On Linux, `cargo pgrx package` can now build RPM and DEB files directly:

* No spec file,
* No `debian/` folder
* No extra tool

You just add an empty section in your `Cargo.toml`:

```
[package]
name = "world"
version = "0.0.0"
description = "Hello World, the Postgres way"
license = "PostgreSQL"
authors = ["Jane Doe <jane@example.com>"]

[package.metadata.rpm]

[package.metadata.deb]
```

And run the usual command:

```
cargo pgrx package --pg-config /usr/pgsql-18/bin/pg_config
```

That's it. The RPM and DEB packages lands in the output directory, next to the staged files.
For the example above, you'd get `world-pg18-0.0.0-1.x86_64.rpm` in `target/release/world-pg18/`,
which will install the extension under `/usr/pgsql-18/`.

The default install paths follow the conventions of the [PGDG](https://www.postgresql.org/download/)
repositories, so your package will feel at home next to the official ones:

Each section builds one package for the Postgres version selected by `pg_config`.
If you want packages for Postgres 15, 16, 17, 18 and 19, just run the command five times with five
different `pg_config`. Your CI will love it.

## Make It Yours

The `description`, `license` and first `authors` entry of your crate fill in the package fields.
If one of them is missing, `cargo pgrx package` will tell you.

And if the defaults don't suit you, any key you set in the section replaces the default value.

For instance:

```
[package.metadata.rpm]
vendor = "ACME Corp."
```

You can also use variables such as `${PG_MAJOR_VERSION}`, `${VERSION}`, `${ARCH}`, `${PG_PKGLIBDIR}`
or `${PG_SHAREDIR}`. The full list and the default specs are in the
[cargo-pgrx README](https://github.com/pgcentralfoundation/pgrx/tree/develop/cargo-pgrx#building-rpm-and-deb-packages).

Note that if you already used `cargo deb`, be careful ! It will also reads `[package.metadata.deb]` but
the metadata parameters may differ. So an existing `cargo deb` metadata may make `cargo pgrx package`
fail. Use `cargo pgrx package --no-distrib-packages` if you want to keep your previous `cargo deb` config

## Why I Wrote This

[PostgreSQL Anonymizer] is built with pgrx, and as I explained in
[a previous post](https://blog.taadeem.net/english/2026/01/25/postgresql_anonymizer_available_near_you),
it is now available in all good shops. Getting there meant building a lot of packages,
for a lot of distributions and a lot of Postgres versions.

I used [nfpm](https://nfpm.goreleaser.com/) for that, which is a great tool. But every pgrx
extension author was solving the same problem on their own, with their own scripts.
It felt like `cargo pgrx` should simply know how to do it.

## Let's Talk!

This is a first version. Package signing is not there yet, and other formats like APK or Arch Linux
packages could follow.

If you maintain a pgrx extension, please give it a try and tell me what's missing.

Open an issue on the [pgrx repository](https://github.com/pgcentralfoundation/pgrx/issues)
or ping me ([@daamien](https://github.com/daamien)).

Happy packaging!

