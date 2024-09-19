---
date: 2024-09-24 10:17:36
layout: post
title: "Writing a Postgres Extension in Rust #1 - A New World"
description: "a new series of articles dedicated to the development of PostgreSQL extensions in general and the [PGRX] framework in particular"
tags: [ PostgreSQL, Rust, extension, extensions_pgrx]
category: english
---

*Eymoutiers, September 23, 2024*

We are launching a new series of articles dedicated to the development of PostgreSQL extensions
in general and the [PGRX] framework in particular. Each article will be designed as a mini-tutorial 
with theoretical reminders, objectives, a concrete example, and optional exercises for readers who 
want to go further!

Let's start with a classic "Hello World"!

<!--MORE-->

![Photo Credit Jacob Colvin]({{ site.baseurl }}/img/2024_pgrx_pexels-jacob-colvin-1761279.jpg)

### Objectives

In this first part, we have the following objectives:

* Initialize a [PGRX] development environment
* Create a minimalist extension
* Run the extension in a Postgres instance

[PGRX]: https://github.com/pgcentralfoundation/pgrx

### Getting Started

> Tip: You can avoid permanently installing Rust and PGRX on the host machine, 
> see the next chapter [Getting Started with Docker]()

First of all, we will install the [PGRX] framework. For this, you must first 
set up Rust (for example with the [rustup] installer).

Once `Rust` and `cargo` are installed, we start the installation of [PGRX] 
and the initialization of the development environment:

``` console
cargo install --locked cargo-pgrx
cargo pgrx init
```

Depending on the power of the host machine, this step can be long (5 to 30 minutes).
It will also consume a lot of disk space (about 4GB).

[rustup]: https://rustup.rs/

### Getting Started with Docker

> This chapter replaces the previous one :)

You can directly perform the examples below by connecting to a Docker machine 
specially prepared for this purpose:

``` console
docker run -it --volume `pwd`:/pgrx daamien/pgrx
```

### Hello World

We can now create an extension named `world` with:

``` console
cargo pgrx new world
```

A new folder `world` has appeared and contains the bare essentials to generate a Postgres extension, 
including a source file (`src/lib.rs`) that contains the following `hello_world` function:

``` rust
#[pg_extern]
fn hello_world() -> &'static str {
    "Hello, world"
}
```

The function returns a static value of type `str` (called a `string slice`) which simply 
represents a sequence of UTF-8 characters. This return value will be automatically 
converted to the `TEXT` type for Postgres.

Thanks to PGRX, all essential Postgres types are automatically converted to Rust types. 
The complete list of conversions is available below:

<https://github.com/pgcentralfoundation/pgrx#mapping-of-postgres-types-to-rust>


### Running the Extension

[PGRX] will now be able to launch a Postgres instance, load this new extension, 
create a test database, and open a working session on it:

``` console
cd world
cargo pgrx run
```

Once the compilation is complete, we are connected to a Postgres instance! 
We can directly create and test our new extension.

``` sql
world=# CREATE EXTENSION world;
CREATE EXTENSION

world=# SELECT hello_world();
 hello_world
--------------
 Hello, world
(1 row)
```

It could not be much simpler, right?

### Adding a New Function

In the `world/src/lib.rs` file, we can add as many new functions as necessary.

For example, let's create a function that takes a parameter as input:

``` rust
#[pg_extern]
fn hello(name: &str) -> String {
    format!("Hello, {name}")
}
```

> Note: For convenience, we use the `String` type here rather than `&str` for the
> return value… The String type will also be converted to the TEXT type for
> Postgres

Let's run the extension again:

``` console
cargo pgrx run
```

> This time, only the `world` module is recompiled!

Once in the new open psql session, we reload the extension:

``` sql
world=# DROP EXTENSION world;
world=# CREATE EXTENSION world;
```

And the new function is available:

``` sql
world=# SELECT hello('Bob');
   hello
------------
 Hello, Bob
(1 row)
```

### Writing the Same Thing in C?

For comparison, here are 2 similar extensions written in C:

<https://github.com/magnusp/pg_hello/blob/master/pg_hello.c>

<https://github.com/petr-korobeinikov/postgresql-extension-example/blob/master/hello_ext/hello_ext.c>


### Going Further…

Here are some activity suggestions to continue the discovery:

* Using the [panic!] macro, create a `hello2()` function that returns
  an error if the `name` variable is empty.

* Using the [match] command, say "Bonjour" if the name is Pierre and "Hola"
  if the name is Diego or Maya.

[panic!]: https://doc.rust-lang.org/std/macro.panic.html#examples
[match]: https://doc.rust-lang.org/rust-by-example/flow_control/match.html

An example implementation is available at:

<https://gitlab.com/daamien/pgrx-notebook/-/blob/main/world/src/lib.rs>

### Conclusion

In a few minutes, we were able to create a Postgres extension to write and test
internal functions.

If we compare it with the classic C extension development framework ([PGXS]), 
we already notice several clear advantages:

* The development environment setup is automated
* Creating new functions is very simple
* All basic Postgres types are automatically converted
* String manipulation is much more flexible in Rust

[PGXS]: https://www.postgresql.org/docs/current/extend-pgxs.html

### Stay Tuned for the Next Episode!

How far will Rust take us? See you in episode #2 to find out!

*PS: This article was partially translated using a generative AI. 
[The original version is in French](https://blog.dalibo.com/2024/09/09/ecrire_une_extension_postgres_en-rust_1_un_nouveau_monde.html).
*
