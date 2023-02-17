+++
title = "Compile Time Correctness: Type State"
description = "Compile Time Correctness: Type State"
date = "2023-02-17"

[taxonomies]
categories = ["development"]
tags = ["design_patterns"]
+++

> This post is code heavy, and is best viewed on a large screen.

Recently I have gotten into "compile-time-safe-but-still-ergonomic" API obsession.

This post will demonstrate an implementation of the type state design pattern, where methods are not able to be called until calling them is a valid operation.

Future posts will cover how maintaining compile-time safety *while keeping the API ergonomic* becomes increasingly difficult to maintain, and approaches used to address these burdens.


## Builder API: Runtime Safety vs Compile Time Safety

For starters, let's look at a simple case of creating a compile-time safe API using type state.

We want to create a `Cmd` that contains the following two fields:

```rust
struct Cmd {
    profile: Profile,
    other_field: u32,
}

struct Profile(String);
```

We will compare a runtime safe builder API to a compile-time safe builder API:


* **Compile Time Safe:** `ProfileSelection` is a type parameter, and `builder.build()` is not callable until the profile is set, which alters the builder's type state.


### Runtime Safe

For this API, this is the usage we want to end up with:

```rust
fn main() {
    // Ok!
    let _cmd = CmdBuilder::new(123)
        .with_profile(Profile("profile".into()))
        .build()
        .unwrap();

    // Runtime error!
    let _cmd = CmdBuilder::new(123)
        .build()
        .unwrap();
    // thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: "Cmd profile must be selected"', src/main.rs:11:10
}
```

Notably, `builder.build()` may fail at runtime, but it is "runtime safe" because it isn't possible to have a `Cmd` without unwrapping the result. At compile time, an API consumer is informed that "this is fallible" through a `Result<Cmd, String>`.

This `CmdBuilder` API has the following:

* A `new` function which initializes the profile to a placeholder "unset" value.
* A `with_profile` function which sets the profile.
* A `build` function which fails if the profile has not been set.

Code:

```rust
enum ProfileSelection {
    None,
    Value(Profile),
}

struct CmdBuilder {
    profile_selection: ProfileSelection,
    other_field: u32,
}

impl CmdBuilder {
    fn new(other_field: u32) -> Self {
        Self {
            profile_selection: ProfileSelection::None,
            other_field,
        }
    }

    fn with_profile(mut self, profile: Profile) -> Self {
        self.profile_selection = ProfileSelection::Value(profile);
        self
    }

    fn build(self) -> Result<Cmd, String> {
        // Deconstruct self.
        let Self {
            profile_selection,
            other_field,
        } = self;

        let profile = match profile_selection {
            ProfileSelection::None => return Err("Cmd profile must be selected".into()),
            ProfileSelection::Value(profile) => profile,
        };

        Ok(Cmd {
            profile,
            other_field,
        })
    }
}

#[allow(dead_code)]
struct Cmd {
    profile: Profile,
    other_field: u32,
}

struct Profile(String);
```


### Compile Time Safe

For this API, this is the usage we want to end up with:

```rust
fn main() {
    // Ok!
    let _cmd = CmdBuilder::new(123)
        .with_profile(Profile("profile".into()))
        .build();

    // Compile error!
    let _cmd = CmdBuilder::new(123).build();
    //    |                         ^^^^^ method not found in `CmdBuilder<ProfileSelectionNone>`
    // ...
    // 15 | struct CmdBuilder<ProfileSelection> {
    //    | -------------------------------------- method `build` not found for this struct
    //    |
    //    = note: the method was found for
    //            - `CmdBuilder<ProfileSelectionValue>`
}
```

In this case, `builder.build()` is not fallible -- it returns a `Cmd`  instead of a `Result<Cmd, _>`.

This `CmdBuilder` API has the following:

* A `new` function which exists for an unset profile type state.
* A `with_profile` function which sets the profile for that type state.
* A `build` function cannot even be called if the profile isn't set.
* `with_profile` cannot be called twice, making it impossible for overwrites.

With this approach, an API consumer does not have to propagate the error up to the user, because *there is none*.

Code:

```rust
// Type states
struct ProfileSelectionNone;
struct ProfileSelectionValue(Profile);

struct CmdBuilder<ProfileSelection> {
    profile_selection: ProfileSelection,
    other_field: u32,
}

impl CmdBuilder<ProfileSelectionNone> {
    fn new(other_field: u32) -> Self {
        Self {
            profile_selection: ProfileSelectionNone,
            other_field,
        }
    }

    fn with_profile(self, profile: Profile) -> CmdBuilder<ProfileSelectionValue> {
        let Self {
            profile_selection: _,
            other_field,
        } = self;

        let profile_selection = ProfileSelectionValue(profile);
        CmdBuilder {
            profile_selection,
            other_field,
        }
    }
}

impl CmdBuilder<ProfileSelectionValue> {
    fn build(self) -> Cmd {
        // Deconstruct self.
        let Self {
            profile_selection: ProfileSelectionValue(profile),
            other_field,
        } = self;

        Cmd {
            profile,
            other_field,
        }
    }
}

#[allow(dead_code)]
struct Cmd {
    profile: Profile,
    other_field: u32,
}

struct Profile(String);
```


## More Variants

Seems nice. What if there are more variants to selecting a profile?

The following shows what it would be like to add more variants while keeping compile-time safety:

1. First, we need to add another type state type.

    ```diff
     // Type states
     struct ProfileSelectionNone;
     struct ProfileSelectionValue(Profile);
    +/// Use the last used profile
    +struct ProfileSelectionLastUsed;
    ```

2. Next, add behaviour to the relevant `impl` blocks:

    ```rust
    impl CmdBuilder<ProfileSelectionNone> {
        // fn new ..

        // fn with_profile ..

        fn with_profile_last_used(self) -> CmdBuilder<ProfileSelectionLastUsed> {
            let Self {
                profile_selection: _,
                other_field,
            } = self;

            let profile_selection = ProfileSelectionLastUsed;
            CmdBuilder {
                profile_selection,
                other_field,
            }
        }
    }

    impl CmdBuilder<ProfileSelectionValue> {
        fn build(self) -> Cmd {
            // ..
        }
    }

    impl CmdBuilder<ProfileSelectionLastUsed> {
        fn build(self, last_used_values: &LastUsedValues) -> Cmd {
            // Deconstruct self.
            let Self {
                profile_selection: ProfileSelectionLastUsed,
                other_field,
            } = self;

            let profile = last_used_values.profile().clone();

            Cmd {
                profile,
                other_field,
            }
        }
    }
    ```

3. The API consumer now has a different `build` method signature when a last used profile is selected:

    ```rust
    fn main() {
        // Ok!
        let _cmd = CmdBuilder::new(123)
            .with_profile(Profile("profile".into()))
            .build();

        // Ok!
        let last_used_values = LastUsedValues::new(Profile("profile".into()));
        let _cmd = CmdBuilder::new(123)
            .with_profile_last_used()
            .build(last_used_values);
    }
    ```

    In a way, this syntactically looks like method overloading.


## Joy

A nice benefit from compile time safe APIs is, the compiler / LSP shows the API consumer what methods can be called from a particular type state. This means even if there is no internet access, one is not lost!

Instead of every error making you wonder, "what's going on?", every error is the right step forward.


## Problems

While this pattern is great at reducing bugs, applying this pattern to many parameters on the builder is syntax-heavy.

For example, if there are three fields that must all be set before `build()` can be called, then `CmdBuilder` can either have:

* One type parameter, with `N1 * N2 * N3` different structs as type state:

    ```rust
    // `ParamTs`es
    struct ParamOneUnset_ParamTwoUnset_ParamThreeUnset;
    struct ParamOneSet_ParamTwoUnset_ParamThreeUnset(ParamOneValue);
    struct ParamOneUnset_ParamTwoSet_ParamThreeUnset(ParamTwoValue);
    struct ParamOneSet_ParamTwoSet_ParamThreeUnset(ParamOneValue, ParamTwoValue);
    // ..

    struct CmdBuilder<ParamTs> { /* .. */ }
    ```

    This leads to a quadratic number of `impl` blocks. *shudders*

    Quite unwieldly.


* A type parameter for each parameter:

    ```rust
    struct ParamOneUnset;
    struct ParamOneSet(ParamOneValue);
    struct ParamTwoUnset;
    struct ParamTwoSet(ParamTwoValue);
    struct ParamThreeUnset;
    struct ParamThreeSet(ParamThreeValue);
    // ..

    struct CmdBuilder<ParamOneTs, ParamTwoTs, ParamThreeTs> { /* .. */ }
    ```

    This leads to.. ugly type parameter propagation for `CmdBuilder`, should it ever need to be named:

    ```rust
    fn takes_cmd_builder<ParamOneTs, ParamTwoTs, ParamThreeTs>(
        builder: CmdBuilder<ParamOneTs, ParamTwoTs, ParamThreeTs>,
    ) {}
    ```

    <small>You may prefer type parameter names to all be uppercase acronyms, though I find it more comprehendable to have them named with words.</small>


## Ending Note

I'd really like to uphold the principle of "if it compiles, it works", because it is such a joy to use. Engineering that kind of empathy into code can be difficult, but with Rust, at least it is possible.
