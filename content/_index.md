+++
title = "Peace"
sort_by = "weight"
+++

**Peace** is a framework to build robust, *understandable* software automation, incrementally.

# The Peace Advantage

## Constrained Code

<div>

{% card(id="constrained_code") %}
```rust
let current = /* item.read() */;
let desired = /* config value / clean up */;
let diff    = /* desired - current */;

match diff {
    Self::InSync  => { /* do nothing */ }
    Self::Partial(p) => amend(p)?,
    Self::Full |
    Self::Unknown => current.apply(diff)?,
}
```

*⛓️ It is these constraints that give us freedom*

{% end %}

{% card(id="constrained_code_desc") %}
Compile time constraints <small>(with all features)</small> means correctness, safety, and ease-of-use by design:

* For every item creation, there is a clean up.
* Every command has a target state, so *multiple executions is not dangerous*.
* Item states must be:
    - **Fetchable:** For idempotence, this allows automation to skip to where it stopped.
    - **Presentable:** In short for comprehension, in detail for informed decisions.
* Constraints enable Peace to provide toggles between command line and web targets.
{% end %}

</div>


## 🚧 Incremental Maturity

<div>

{% card(id="constrained_code", fixed_width=true) %}
```toml
[dependencies.peace]
version = "0.0.?"
features = [
    "error_reporting",
    "output_colorized",
    "output_json",
    "output_progress",
    "state_current",  # 🚧
    "state_desired",  # 🚧
    "state_diff",     # 🚧
    # ..
]
```
{% end %}

{% card(id="constrained_code_desc") %}
🐚 Instead of writing shell scripts, then transitioning to larger tools as complexity grows, existing logic is extended by enabling features in the Peace crate.

```diff
 async fn exec(
     state_current: Self::State,
     state_desired: Self::State,
+    state_diff: Self::StateDiff,
 ) -> Result<(), E>
```

🦀 *Compilation errors* inform you what code that needs updating.

{% end %}

</div>

## Adaptive Presentation

<div>

{% card(id="adaptive_presentation", fixed_width=true) %}

<div id="adaptive_presentation_dot"></div>

<pre class="terminal">
<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>apply</span>  <span style='color:#7f7f7f'># interactive</span>
<span style='color:#5f87ff'>app   </span>▕<span style='background:#00af5f'>             </span>▏built in 4s
<span style='color:#5f87ff'>server</span>▕<span style='background:#0087d7'>    </span><span style='background:#00005f'>         </span>▏el: 7s, eta: 9s
<span style='color:#5f87ff'>upload</span>▕<span style='background:#000033'>             </span>▏deps: [server]
</pre>

<pre class="terminal">
<span style='color:#7f7f7f'># ci</span>
server | 2/5 | 0s | os boot
server | 2/5 | 7s | os booted
</pre>

{% end %}

{% card(id="adaptive_presentation_desc") %}
Peace handles the information and command presentation based on usage.

* Progress presented as bars when used interactively, and single line logs in CI.
* 🚧 JSON when returned as a web response.
* 🚧 HTML elements for rendering -- WASM / web.
* 🚧 Forms for controls in a browser.

{% end %}

</div>


## Understandable Errors

Peace is compatible with `miette` by design, presenting errors in the much loved and understood format.

🚧 Currently only a few error variants have source information automatically attached.



* **Discovery:** Show the current state of all items, and what they would be.
* **Subset Execution:** Run the automation for these items.
* **Reverse Execution:** Revert these items to the previous state.
* **Clean Up:** Track all items that are created, and clean the up.
* **Interface:** Expose the logic as a command line tool, or web API, or web page.


# Building Blocks / Concepts

## Item Specification

<div>
{{ card(id="item_spec_dot", fixed_width=true) }}
{% card(id="item_spec_desc") %}

A collection of functions to manage an *item*, e.g. application compilation, file upload, server existence.

Base functions:

* **Current:** Retrieve the current state.
* **Desired:** Retrieve the desired state.
* **Diff:** Diff the current and desired states.
* **Apply:** Change the state from current to desired.
* **Clean:** Clean up the item.
* more to come

{% end %}
</div>


## Item Specification Graph

<div>
{{ card(id="item_spec_graph_dot", fixed_width=true) }}
{% card(id="item_spec_graph_desc") %}

A [directed graph](https://en.wikipedia.org/wiki/Directed_graph) of **Item Spec**s. Edges indicate forward execution order.

Functions can be executed in parallel when their predecessors' executions are complete.

{% end %}
</div>


## Flows

<div>
{{ card(id="flows_dot", fixed_width=true) }}

{% card(id="flows_desc", fixed_width=true) %}

<pre class="terminal">
<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>status</span>
Using profile <span style='color:#5fff87'>development</span>
<span style='color:#5f87ff'>app</span>: `web_app` out-of-date
<span style='color:#5f87ff'>server</span>: not exists
<span style='color:#5f87ff'>upload</span>: `web_app` not uploaded

<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>diff</span>
<span style='color:#5f87ff'>app</span>: `web_app` will be compiled
<span style='color:#5f87ff'>server</span>: will be launched
<span style='color:#5f87ff'>upload</span>: `web_app` will be uploaded

<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>apply</span>
<span style='color:#5f87ff'>app   </span>▕<span style='background:#00af5f'>             </span>▏built in 4s
<span style='color:#5f87ff'>server</span>▕<span style='background:#0087d7'>    </span><span style='background:#00005f'>         </span>▏el: 7s, eta: 9s
<span style='color:#5f87ff'>upload</span>▕<span style='background:#000033'>             </span>▏deps: [server]

For detail, in another terminal run:
<span style='color:#5fff87'>envman</span> <span style='color:#cccccc'>progress --detail</span>
</pre>

{% end %}

A flow is a process that fulfills a business question or intent. Within a flow, the appropriate Item Specification *function*(s) are invoked for each *item*.

Examples:

* Discover and show the current state of all items.
* Show what would change when the automation is executed.
* Execute the automation.

Advanced flows may reverse the execution direction. Examples:

* `clean` cleans up items in reverse order.
* `apply` on a *subset* of items may transition some items to the desired state, and clean others.

</div>

# The Peace Disadvantage

*Reasons you would **not** use Peace*

1. Higher levels of automation maturity implies higher cost of developing the automation

    Low maturity automation can evolve faster as it has fewer constraints. This is sustainable when the number of users is not high and the cost of higher resilience and usability is not justified.

2. Rust is not as common a skillset as other languages, and may be more difficult to hire for.



# Roadmap

* Idempotent: Run the same command twice
* Colorized output
* Progress bars
* Vertically-sliced functionality by design via crate features
* Versioning state.


---

# Inspirations

## Rust

Rust's compilation errors are presented in a way that makes the users feel understood. Users don't feel lost when their input is wrong; they feel lost when they don't have a way to move forward.


## Git

Git allows one to show the state of local and remote repositories, diff them, before committing and pushing the changes. Pushes are atomic and idempotent, meaning running the same command multiple times, whether or not the operation succeeded prior, has the same result.

The idea spawned from this is, instead of file content being the state, and patches being the diff, why not something -- *anything* -- user defined?


## People

Vinh Giang is a keynote speaker and excellent communicator. Through his communications course, I realized that a lot of issues with automation can be solved by communicating clearly and understandably with the end users.

<script type="module">
    import { Graphviz } from "https://cdn.jsdelivr.net/npm/@hpcc-js/wasm/dist/graphviz.js";

    const graphviz = await Graphviz.load();
    const item_spec_dot = `digraph G {
        graph [
            penwidth  = 0
            nodesep   = 0.5
            ranksep   = 0.6
            bgcolor   = "transparent"
            fontcolor = "#7f7f7f"
            splines   = line
            rankdir   = LR
        ]
        node [
            penwidth  = 2
            fontcolor = "#111111"
            fontname  = "helvetica"
            fontsize  = 12
            shape     = "rectangle"
            style     = "filled,rounded"
            margin    = 0.0
            color     = "#9999aa"
            fillcolor = "#ddddf5"
        ]
        edge [
            penwidth  = 2
            arrowsize = 0.7
            color     = "#7f7f7f"
            fontcolor = "#7f7f7f"
        ]

        a [
            shape="none"
            bgcolor="ddddf5"
            label = <<table
                style="rounded"
                border="2"
                cellborder="0"
                cellpadding="7"
                cellspacing="0"
                >
            <tr>
                <td colspan="2"><b>a:</b>&nbsp;Application Compilation</td>
            </tr>
            <hr/>
            <tr>
                <td align="left" balign="left" bgcolor="#ddddaa">Current State</td>
                <vr />
                <td align="left" balign="left">⚫ Not exists<br/>🔵 Out-of-date<br/>⚫ Up-to-date</td>
            </tr>
            <hr/>
            <tr>
                <td align="left" balign="left" bgcolor="#aaddaa">Desired State</td>
                <vr />
                <td align="left" balign="left">⚫ Not exists<br/>⚫ Out-of-date<br/>🔵 Up-to-date</td>
            </tr>
            <hr/>
            <tr>
                <td align="left" balign="left" bgcolor="#88cc88">Apply Diff</td>
                <vr />
                <td align="left" balign="left">Application will be recompiled<br/>to 'target/debug/app'</td>
            </tr>
            <hr/>
            <tr>
                <td align="left" balign="left" bgcolor="#88cc88">Apply Exec</td>
                <vr />
                <td align="left" balign="left">Run <font face="monospace" point-size="12">cargo build</font></td>
            </tr>
            <hr/>
            <tr>
                <td align="left" balign="left" bgcolor="#ccaadd">Clean State</td>
                <vr />
                <td align="left" balign="left">🔵 Not exists<br/>⚫ Out-of-date<br/>⚫ Up-to-date</td>
            </tr>
            <hr/>
            <tr>
                <td align="left" balign="left" bgcolor="#bb88cc">Clean Diff</td>
                <vr />
                <td align="left" balign="left">'target/debug/app' will be deleted</td>
            </tr>
            <hr/>
            <tr>
                <td align="left" balign="left" bgcolor="#bb88cc">Clean Exec</td>
                <vr />
                <td align="left" balign="left">Run <font face="monospace" point-size="12">cargo clean</font></td>
            </tr>
        </table>>]
    }`;
    // const item_spec_svg = graphviz.dot(item_spec_dot);
    document.getElementById("item_spec_dot").innerHTML = graphviz.layout(item_spec_dot, "svg", "dot");

    const item_spec_graph_dot = `digraph G {
        graph [
            penwidth  = 0
            nodesep   = 0.5
            ranksep   = 0.6
            bgcolor   = "transparent"
            fontcolor = "#7f7f7f"
            splines   = line
            rankdir   = LR
        ]
        node [
            penwidth  = 3
            fontcolor = "#111111"
            fontname  = "monospace"
            fontsize  = 12
            shape     = "circle"
            style     = "filled"
            width     = 0.6
            height    = 0.6
            margin    = 0.04
            color     = "#9999aa"
            fillcolor = "#ddddf5"
        ]
        edge [
            penwidth  = 2
            arrowsize = 0.7
            color     = "#7f7f7f"
            fontcolor = "#7f7f7f"
        ]

        a [label = <<b>a</b>>]
        b [label = <<b>b</b>>]
        c [label = <<b>c</b>>]
        d [label = <<b>d</b>>]
        e [label = <<b>e</b>>]

        a -> b
        a -> c
        b -> d
        b -> e
        c -> e
    }`;
    document.getElementById("item_spec_graph_dot").innerHTML = graphviz.layout(item_spec_graph_dot, "svg", "dot");

    const flows_dot = `digraph G {
        graph [
            penwidth  = 0
            nodesep   = 0.1
            ranksep   = 0.3
            bgcolor   = "transparent"
            fontname  = "helvetica"
            fontsize  = 12
            fontcolor = "#9f9f9f"
            splines   = line
            rankdir   = LR
        ]
        node [
            penwidth  = 4
            fontcolor = "#111111"
            fontname  = "monospace"
            fontsize  = 9
            shape     = "circle"
            style     = "filled"
            width     = 0.25
            height    = 0.25
            margin    = 0.04
            color     = "#9999aa"
            fillcolor = "#ddddf5"
        ]
        edge [
            penwidth  = 1
            arrowsize = 0.5
            color     = "#7f7f7f"
            fontcolor = "#7f7f7f"
        ]

        subgraph cluster_apply_exec {
            label = "apply"

            node [color="#449966" fillcolor="#88cc88"]

            apply_exec_a [label = ""]
            apply_exec_b [label = ""]
            apply_exec_c [label = ""]
            apply_exec_d [label = "" fillcolor="#ddddaa" style="dashed,filled"]
            apply_exec_e [label = "" fillcolor="#ddddaa" style="dashed,filled"]

            apply_exec_a -> apply_exec_b
            apply_exec_a -> apply_exec_c
            apply_exec_b -> apply_exec_d
            apply_exec_b -> apply_exec_e
            apply_exec_c -> apply_exec_e
        }

        subgraph cluster_clean_exec {
            label = "clean"

            node [color="#884499" fillcolor="#ccaadd"]
            edge [dir = back]

            clean_exec_a [label = "" color="#aaaa55" fillcolor="#ddddaa"]
            clean_exec_b [label = "" fillcolor="#ddddaa" style="dashed,filled"]
            clean_exec_c [label = "" fillcolor="#ddddaa" style="dashed,filled"]
            clean_exec_d [label = ""]
            clean_exec_e [label = ""]

            clean_exec_a -> clean_exec_b
            clean_exec_a -> clean_exec_c
            clean_exec_b -> clean_exec_d
            clean_exec_b -> clean_exec_e
            clean_exec_c -> clean_exec_e
        }

        subgraph cluster_state_current {
            label = "current state"

            node [color="#aaaa55" fillcolor="#ddddaa"]

            state_current_a [label = ""]
            state_current_b [label = ""]
            state_current_c [label = ""]
            state_current_d [label = ""]
            state_current_e [label = ""]

            state_current_a -> state_current_b
            state_current_a -> state_current_c
            state_current_b -> state_current_d
            state_current_b -> state_current_e
            state_current_c -> state_current_e
        }

        subgraph cluster_state_desired {
            label = "desired state"

            node [color="#449966" fillcolor="#ddddaa"]

            state_desired_a [label = ""]
            state_desired_b [label = ""]
            state_desired_c [label = ""]
            state_desired_d [label = ""]
            state_desired_e [label = ""]

            state_desired_a -> state_desired_b [weight = 4]
            state_desired_a -> state_desired_c [weight = 3]
            state_desired_b -> state_desired_d
            state_desired_b -> state_desired_e
            state_desired_c -> state_desired_e
        }

        subgraph cluster_state_clean {
            label = "clean state"

            node [color="#884499" fillcolor="#ddddaa"]
            edge [dir = back]

            state_clean_a [label = ""]
            state_clean_b [label = ""]
            state_clean_c [label = ""]
            state_clean_d [label = ""]
            state_clean_e [label = ""]

            state_clean_a -> state_clean_b [weight = 4]
            state_clean_a -> state_clean_c [weight = 3]
            state_clean_b -> state_clean_d
            state_clean_b -> state_clean_e
            state_clean_c -> state_clean_e
        }

        state_current_a -> state_desired_a [style = "invis"]
        state_current_a -> state_clean_a [style = "invis"]
        state_desired_d -> apply_exec_a [style = "invis"]
        state_desired_e -> apply_exec_a [style = "invis"]
        state_clean_d -> clean_exec_a [style = "invis"]
        state_clean_e -> clean_exec_a [style = "invis"]
    }`;
    document.getElementById("flows_dot").insertAdjacentHTML(
        "afterbegin",
        graphviz.layout(flows_dot, "svg", "dot")
    );

    const adaptive_presentation_dot = `digraph G {
        graph [
            penwidth  = 0
            nodesep   = 0.0
            ranksep   = 0.3
            bgcolor   = "transparent"
            fontcolor = "#7f7f7f"
            splines   = line
            rankdir   = LR
        ]
        node [
            penwidth  = 3
            fontcolor = "#111111"
            fontname  = "monospace"
            fontsize  = 12
            shape     = "circle"
            style     = "filled"
            width     = 0.3
            height    = 0.3
            margin    = 0.04
            color     = "#9999aa"
            fillcolor = "#ddddf5"
        ]
        edge [
            penwidth  = 2
            arrowsize = 0.7
            color     = "#7f7f7f"
            fontcolor = "#7f7f7f"
        ]

        subgraph cluster_a {
            a [color="#449966" fillcolor="#88cc88" label = <<b>a</b>>]
            a_text [shape="plain" style="none" fontcolor="#eeeeff" label = <app>]
        }
        subgraph cluster_b {
            margin = 0

            b [color="#446699" fillcolor="#88aacc" label = <<b>b</b>>]
            b_text [shape="plain" style="none" fontcolor="#eeeeff" label = <server>]
        }
        subgraph cluster_c {
            c [label = <<b>c</b>>]
            c_text [shape="plain" style="none" fontcolor="#eeeeff" label = <upload>]
        }

        b_tooltip [
                penwidth  = 2
                shape     = "rectangle"
                style     = "rounded,filled"
                color     = "#446699"
                margin    = 0.1
                fontname  = "helvetica"
                fillcolor ="#ccccdd"
                label = <<b>Elapsed:</b>&nbsp;&nbsp;7s<br align="left"/>
<b>ETA:</b>&nbsp;9s<br align="left"/>
<b>Message:</b>&nbsp;OS booted<br align="left"/>>
            ]

        a -> b
        b -> c

        a -> b_tooltip [style = "invis"]
    }`;
    document.getElementById("adaptive_presentation_dot").innerHTML = graphviz.layout(adaptive_presentation_dot, "svg", "dot");
</script>
