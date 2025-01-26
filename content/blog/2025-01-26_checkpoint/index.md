+++
title = "Checkpoint"
description = "Checkpoint"
date = "2025-01-26"

[taxonomies]
categories = ["development", "community"]
tags = ["direction"]
+++

Does this project have to be an endless single player game?


## TL;DR

I'm hoping for any combination of the following:

1. People who want to see the Peace framework developed to viable-existence, to [fund my existence][github_sponsors].
2. Someone to take over and continue development of the Peace project.
3. Someone who finds the [✒️ `dot_ix`][dot_ix_web] side-side project fun or fulfilling, to take over it / develop [`layout-rs`][layout_rs] as a replacement for GraphViz dot, and switch to [`encre-css`][encre_css] for styling.


## 1. Motivation

Each time I provide or use a tool that manages software infrastructure, there is always something lacking. Pragmatically, an organisation whose business is to ship a product, cannot invest disproportional resource on improving tools beyond "it's efficient enough".

However, one of my desires is to have ridiculously good tools<sup>1</sup>, because they can:

* switch dangerous buttons for safe ones
* simplify overwhelming information into someting understandable
* turn fear of mistakes into fun of learning

<sup>1</sup> <small>ridiculous enough for a dreary-eyed 🦹 engineer to get excited</small>


## 2. Demo

`envman` is a tool built using the Peace framework that:

1. Downloads a file from Github.
2. Creates some AWS resources.
3. Uploads the file to AWS.

The videos below show the beginnings of "ridiculously good".


### 2.1. Like Git, But With Infrastructure

<video controls="controls">
    <source src="2025-01-26_envman_simple_cycle.mp4" type="video/mp4" />
    <object data="2025-01-26_envman_simple_cycle.mp4" ></object>
</video>

Regardless of the process, a really nice tool would be able to do the following:

1. Tell me what the current state of a system is.
2. Tell me what will happen, if I were to run the automation.
3. Don't create twice the resources when I run it twice.
4. Don't clutter my screen with irrelevant detail.
5. *Do* tell me the detail, when I care<sup>2</sup>.
6. Make sure everything is cleaned up.

These capabilities (among others) allow me to do what I want without needing to worry about making mistakes, or forgetting to clean up resources.

<sup>2</sup> <small>Not shown, but detailed state is stored</small>


### 2.2. Interruptible

<video controls="controls">
    <source src="2025-01-26_envman_interrupt.mp4" type="video/mp4" />
    <object data="2025-01-26_envman_interrupt.mp4" ></object>
</video>

If I change my mind, let me stop the process (safely!), change a parameter, and resume the process.

And don't throw away all the work that was done before -- determine what's there, and only change the parts that need changing.


### 2.3. Profiles (like Branches)

<video controls="controls">
    <source src="2025-01-26_envman_multi_profile.mp4" type="video/mp4" />
    <object data="2025-01-26_envman_multi_profile.mp4" ></object>
</video>

Sometimes I work with multiple environments, so let me switch between them.

And let me compare the state of two environments -- perhaps one is a replica of the other, so show me what's changed.


### 2.4. Communicate Clearly, Recover Cleanly

<video controls="controls">
    <source src="2025-01-26_envman_disconnect.mp4" type="video/mp4" />
    <object data="2025-01-26_envman_disconnect.mp4" ></object>
</video>

When something goes wrong, clearly showing which step went wrong (the red one!) and how to recover saves both time and effort in continuing in the flow.

Bonus points for recovering by simply pressing the "go" button again.


### 2.5. Visual

> 🚧 work-in-progress feature

<video controls="controls">
    <source src="2025-01-26_envman_web.mp4" type="video/mp4" />
    <object data="2025-01-26_envman_web.mp4" ></object>
</video>

Not everyone works on the command line, and even for the software-inclined, being able to *see* what each step of the process does, without running the automation, helps build understanding faster than simply seeing a list of steps.

Having real-time visibility of automation execution makes it clear which resources are interacting.

<video controls="controls">
    <source src="2025-01-26_envman_web_error.mp4" type="video/mp4" />
    <object data="2025-01-26_envman_web_error.mp4" ></object>
</video>

If there is an error, appropriate styling immediately draws attention to the resources are involved in the error.


---

Skip ahead to [5. Imagination](#5-imagination) if you don't care how it's built.

---

## 3. Distributing Effort

To build tools of this calibre everywhere, practically one would have to separate:

1. Logic common to items managed by automation (e.g. save / load / display information).
2. Logic to manage a particular item (e.g. download a file).
3. Parameters / values used in the automation (e.g. use this particular URL).
4. The combining of these parts (organisation specific process / intellectual property).

That way the overall goodness can be provided by the framework, automation logic is extendable and shareable, and everyone gets to keep their secrets.

<object
    type="image/svg+xml"
    data="peace_framework_usage.svg"
    width="700"
    ></object>
<br/>
<small><a href="https://azriel.im/dot_ix/#src=MQAgogHghgtgDgGwKYC4QGUAuBPZBnEACjwEt5kBKAKAFo6qALEpAJyhYGMHsUqQQoAV0wB7GFEwkRAOwD6JTEhh40AbwC+fEHCRQOqLfx16ksuCxEA3EgBMkN2QhEBzEhzWb+R3ftkAzNhgkAHcRFgBrWUw2BRUQDS1REQQPKippETtZaVgkFS1jfTQAIkBeDcApXcB4P5AABR8kEAAxQJCw8OKC+rMLazsHJ1d3EGK6k20e23sQAbcO7xN-FtCIqJjMOJH6kAAVNbw5gWExCSk5BSUN0sAtnZAAQSPxSRkQAElFZQOklOHATLJq+9Ej1OOxEyQ66UypjseA4+X4QkBJxk8necQAPjRDEQ7JYQAB6EAcMQwQTSBTYEB2PwkaT2aheEBIAB0ziZWJoTRIyAAIiJgtInFAbOyQAA1EgsTCCKAIACyeiYtK0nQW5isk36LjcaAxWMIRJgMGeflJHCeOQQ5PpXmZrJFLzs8BEimkmEZECQHGEpxF6CgfgaNMULBYgjgkgARlzySKAHK5PBwEw2RnSawWaRBV14EUAYWJUGkKcttJAQdYfhMIoA6kgI2XXRWqyrfAFcstItEoLEdZiGdykNTaXCvBz0AwoDoQCI-NMtRwRdyJFBCTI8F2gznEqDvrqGQBGJkgADiFjDBARx3Nc8GWIATEftlBwnkG9p2LlgwQ-GFGQqy+8WIAMxHgAEoWNgEFYrAgKItRbD+LDup63oyGk9jOHkvDwg8SJnKishmF0bZBB2qzdusaAANqXkCyLnMoAA02jEUsbTkbEAC6LamCRrQrBu6yEYUphqr09iOPO1EiYs7bsYJeDMTJYkapJgzcfMvgqX0aluIRXzSV02kSTMHDMV8GmHIi5oohc+k7tRtF4bZTGwTu3FUM4bBwAw1gAF6yBI0QjhhphEtI64xNm2FeE5NkMXgwmsXJAl7GglYIHgSBYspEw6aZ9nJOlMpZVooWyDANLIBFMWaaJeUmfOhXfEBaSYAwSgGPwQQsJhsjBAoDCyBGUBZWg0SCA0oBUkICDrG5sGhtl-DrrgWFYuVM2CHNI4MtSCAILIeATlkGQsOI3wAORAQADDdl1Yvw+2HcdQqmAw0EsGgl13ndD0Mk9XIvSdvEiF6cTXX9j0gM9R0g4FZokJYqAgJD93Q5FIgvnDb3ZGEF3fQArFDAMgJj2OvVkH3I19qO3ejpPk6YlOg+DRMkwDTM41keiSMj7MM5z0RY6YA02O131UXenEPTlyWkfJexJaqDWaoMtVeNYpBRpaOBoDSWskBGyBpAylrOAwmBoAAZOblvQ7DLN4+dMrffuHN7UD3PvZ97P-QDjvwz+bOo+7guewd3sI3zKOXWH-sMlzTtnQTqMACwe14Sfw9TrDffTCdZ8LFPw7zSOx8T6NyyYGv8AAPHXaAAFR25gGMTjoshEk4tPG5NDte8n+Ou6jxOFytxfMzgyBoDYzqKMK1etmxqUUbtXgN83rft5OYXJGEaB99lS-1eq+VSdDm8gC3JAW23jMd3vPeHwg-dYnFpwuev9eN9f28P7vLu+9aZwEECwRAx8GQf3ogRIiCw+JkQUrXMmk8jrTxRnPTAC9TZeAMpfX+N874707t3A+IAvJICQNIKgQA">source</a></small>

Using a general purpose programming language to define these parts means:

* Tooling from that language is available to adhere to best practices.
* The standard packaging system handles publishing and distribution.


### 3.1 Incentive

Peace is intended to be low-friction to uptake<sup>3</sup> -- developers who use Peace should not need to write much additional code than what they would have if they were writing basic automation logic.

It should be a matter of:

1. Creating types for the parameters and state of the item managed by automation.
2. Fitting the automation logic into the [`Item`](https://github.com/azriel91/peace/blob/0.0.14/crate/cfg/src/item.rs) trait.
3. Instead of calling each automation step's function directly, putting that into a graph structure called a `Flow`.
4. Calling the appropriate Peace `Cmd` with the defined `Flow`.

which should be similar effort to writing a tool without the Peace framework.

For the web diagram, ["item interactions"](https://github.com/azriel91/peace/blob/0.0.14/crate/cfg/src/item.rs#L509-L513) need to be defined, which Peace will combine with other `Item`s to create a diagram that contains all items.

In practice, when switching from "just code a quick tool" to "write it using the Peace framework", anecdotally it's switching from 1 unit of effort producing 1 unit of value, to 1.5 units of effort producing 4 units of value.


<sup>3</sup> <small>this statement may not be well-fitting since it's written in Rust, but if you were to have written a small automation tool in Rust, it should be nearly identical effort, for significantly more value delivered</small>


## 4. Side-Side Projects

<object
    type="image/svg+xml"
    data="peace_side_projects.svg"
    width="500"
    ></object>
<br/>
<small><a href="https://azriel.im/dot_ix/#src=MQAgogHghgtgDgGwKYC4QGUAuBPZBnEACjwEt5kBKAKAFo6qALEpAJyhYGMHsUqQQWSPDCgA7NAG8AvnxA44SAPqCA5pJn8AZqMUq2cButklRmViwCucTCQBGyI-wAmAe0yKSERyAVQOqEGkqKlEXJyVRWCFeZzcPLxAAIkAkckB4P5BXd09E41NzKxt7AMTAXg3ARb2QEzMWS2s7ZBytHT0oAzREwD4NwBvdkG1dfQZGuWwFZSQ1JJLAId30+SVVIcFhMXbp9KWRUSHff1XAKV30naQcqiQnFWjZTPjFRSO0AG1rzwAaHyQ-JABdXOrawuQt3uIAeVXydSKbyOPya-VaDCBH12IL6LQMUKR31kczGKkRn0eONUGM+MIEQk2+ORDw2YhJ-h+VEwDCQMFQsjwOHwMX4IFC4UU4U0UAsCEweB5vN6JAQCEUeAYUAFoRYIgQaAA5AAmAAMOo1silmhlcoVSqUDBcADdWJqAKz6w2842y+WKgWaFwcCwSkDavUGqVaE1u82KPw2G2a3WOoMgTksFwAayUZuVLlVUHVfoALAGnfwE8nU+6LdbbX6AMz5uNFlOhj1en2avOxoN1kthiMkKN+h1t-hOs4XQVIYWi8WS50htMRDNqls1oMu02lxSWm0sTXVwPLmdrz3e30a1u7o37rscSMBE9LqUdhtzzPZjX9s+8h+z9flre5u8fzBE3rL9D2bPt-0LQDi0fcMrx7G8ADZ8yde4C3jRVRg4FwEAzNB7AsJAgA">source</a></small>

In building this project, besides [restarting](https://github.com/azriel91/nginee) [twice](https://github.com/azriel91/choochoo), a number of side, side projects were created. The ones that are likely useful on their own are:

* [✒️ `dot_ix`][dot_ix]: Interactive dot graphs (diagrams).
* [🛑 `interruptible`](https://github.com/azriel91/interruptible): Stops a future producer or stream from producing values when interrupted.
* [🧬 `fn_graph`](https://github.com/azriel91/fn_graph): Runs interdependent logic concurrently, starting each function when predecessors have completed.
* [🗂️ `type_reg`](https://github.com/azriel91/type_reg): Serializable map of any type.
* [🗂️ `resman`](https://github.com/azriel91/resman): Runtime managed resource borrowing.

So in order for Peace to fulfill the "ridiculously good" notion, many side quests just had to be done.


### 4.1 Dot IX

`dot_ix` in particular has its own [client side application][dot_ix_web] to create SVG diagrams from structured input. I frequently use this to create various technical diagrams.

My wishlist for [`dot_ix`][dot_ix]:

1. For diagram generation, use [`layout-rs`][layout_rs] to generate the SVG in-process instead of delegating to a WASM-compiled GraphViz `dot`.

    Besides performance gains and portability, it may be possible to rasterize the SVG and render a diagram in the terminal.

    However, [`layout-rs`][layout_rs] only supports basic text labels, so a lot more development effort needs to be poured into it to unlock this possibility.

2. For styling, instead of using [`tailwindcss`](https://tailwindcss.com/)'s non-production CDN JS and [hacky CSS extraction](https://github.com/azriel91/dot_ix/blob/0.9.1/crate/web_components/src/dot_svg/svg_write_to_clipboard.js#L12-L16), switch to [`encre-css`][encre_css].

    Perhaps update `encre-css` to synchronize with Tailwind CSS 4.0 as well.

It's too far down the priority list for myself to get to in the foreseeable future, but if someone were to do them I'll be grateful.

[dot_ix]: https://github.com/azriel91/dot_ix
[dot_ix_web]: https://azriel.im/dot_ix/
[layout_rs]: https://crates.io/crates/layout-rs
[encre_css]: https://crates.io/crates/encre-css


## 5. Imagination

There are nice things I imagined being able to do:

1. Visually select the items in the process that I want to run, then press go -- subset execution, with an interactive UI over it.
2. Selecting a node on the web diagram opens a panel with detailed information about that item.
3. Undo the last execution / make the environment as it was *at this particular point*.

Reaching that level of simplicity may take a few thousand additional hours of effort.


## 6. Summary

Chipping away at this for 5 years, it certainly feels too much for one person.

I had a plan to get this [production ready](https://github.com/users/azriel91/projects/1/views/1?query=sort%3Aupdated-desc+is%3Aopen) and use it at work, but being on the screen for both work and home activities doesn't sit well.

So either this becomes what I work on full time (through [sponsorship][github_sponsors]), or someone else who has the energy could take it and build upon what's there (and hopefully we share the same level of code-pedantry 🕵️).

Let me know ([discord](https://discord.gg/R6HcASbYAj) / email: `mail@azriel.im`).

[github_sponsors]: https://github.com/sponsors/azriel91
