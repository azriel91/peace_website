+++
title = "Peace"
sort_by = "weight"
+++

**Peace** is a framework to build robust, *understandable* software automation, incrementally.

# Building Blocks / Concepts

## Item Specification

<div>
<div id="item_spec_dot" style="display: inline-block; width: 470px; vertical-align: middle;"></div>
<div id="item_spec_desc" style="display: inline-block; max-width: 470px; vertical-align: middle;">

A collection of functions to manage an *item*, e.g. application compilation, file upload, server existence.

Base functions:

* **Current:** Retrieve the current state.
* **Desired:** Retrieve the desired state.
* **Apply:** Change the state from current to desired.
* **Clean:** Clean up the item.
* more to come

</div>
</div>


## Function Graph

<div>
<div id="item_spec_graph_dot" style="display: inline-block; width: 470px; vertical-align: middle;"></div>
<div id="item_spec_graph_desc" style="display: inline-block; max-width: 470px; vertical-align: middle;">

A [directed graph](https://en.wikipedia.org/wiki/Directed_graph) of **Item Spec**s. Edges indicate forward execution order.

Functions can be executed in parallel when their predecessors' executions are complete.

</div>
</div>


## Flows


<div>
<div id="flows_dot" style="display: inline-block; width: 470px; vertical-align: middle;"></div>
<div id="flows_desc" style="display: inline-block; max-width: 470px; vertical-align: middle;">

A process to answer a business level question, such as "what is the current state of all items", or "what would change when the automation is executed".

The appropriate Item Specification *function* can be invoked for each *item*.

Notably the execution direction may be reversed, e.g. `clean` will clean up items in the reverse order they were created in.

</div>
</div>


# Capabilities

* **Discovery:** Show the current state of all items, and what they would be.
* **Subset Execution:** Run the automation for these items.
* **Reverse Execution:** Revert these items to the previous state.
* **Clean Up:** Track all items that are created, and clean the up.
* **Interface:** Expose the logic as a command line tool, or web API, or web page.



# Roadmap

* Idempotent: Run the same command twice
* Colorized output
* Progress bars
* Vertically-sliced functionality by design via crate features



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
            fontsize  = 13
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
            fontsize  = 13
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
            ranksep   = 0.2
            bgcolor   = "transparent"
            fontname  = "helvetica"
            fontsize  = 13
            fontcolor = "#9f9f9f"
            splines   = line
            rankdir   = LR
        ]
        node [
            penwidth  = 3
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
            arrowsize = 0.4
            color     = "#7f7f7f"
            fontcolor = "#7f7f7f"
        ]

        subgraph cluster_apply_exec {
            label = "apply"

            node [color="#449966" fillcolor="#88cc88"]

            apply_exec_a [label = ""]
            apply_exec_b [label = ""]
            apply_exec_c [label = ""]
            apply_exec_d [label = "" color="#aaaa55" fillcolor="#ddddaa"]
            apply_exec_e [label = ""]

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
            clean_exec_b [label = ""]
            clean_exec_c [label = "" color="#aaaa55" fillcolor="#ddddaa"]
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

            node [color="#449966" fillcolor="#88cc88" style="dashed,filled"]

            state_desired_a [label = ""]
            state_desired_b [label = ""]
            state_desired_c [label = ""]
            state_desired_d [label = ""]
            state_desired_e [label = ""]

            state_desired_a -> state_desired_b
            state_desired_a -> state_desired_c
            state_desired_b -> state_desired_d
            state_desired_b -> state_desired_e
            state_desired_c -> state_desired_e
        }

        subgraph cluster_state_clean {
            label = "clean state"

            node [color="#884499" fillcolor="#ccaadd" style="dashed,filled"]
            edge [dir = back]

            state_clean_a [label = ""]
            state_clean_b [label = ""]
            state_clean_c [label = ""]
            state_clean_d [label = ""]
            state_clean_e [label = ""]

            state_clean_a -> state_clean_b
            state_clean_a -> state_clean_c
            state_clean_b -> state_clean_d
            state_clean_b -> state_clean_e
            state_clean_c -> state_clean_e
        }

        state_current_d -> state_desired_a [style = "invis"]
        state_current_e -> state_desired_a [style = "invis"]
        state_desired_d -> apply_exec_a [style = "invis"]
        state_desired_e -> apply_exec_a [style = "invis"]
        state_current_d -> state_clean_a [style = "invis"]
        state_current_e -> state_clean_a [style = "invis"]
        state_clean_d -> clean_exec_a [style = "invis"]
        state_clean_e -> clean_exec_a [style = "invis"]
    }`;
    document.getElementById("flows_dot").innerHTML = graphviz.layout(flows_dot, "svg", "dot");
</script>
