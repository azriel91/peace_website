+++
title = "Peace"
template = "home.html"
insert_anchor_links = "heading"
sort_by = "weight"
+++

> 🚧 **Important:**
>
> Peace is a work in progress, and this website serves to communicate the project vision.
>
> Approximately 10% of the features listed on this page are implemented; a substantial amount of the completed work has been foundational and exploration.

**Peace** is a framework to build robust, *understandable* software automation, incrementally.


# The Peace Advantage

## Visibility

<div class="card_set">

{% card(id="visibility") %}

<pre class="terminal">
<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>status</span>
Using profile <span style='color:#5fff87'>development</span>
<span style='color:#5faaff'>app</span>: `web_app` out-of-date
<span style='color:#5faaff'>server</span>: not exists
<span style='color:#5faaff'>upload</span>: `web_app` not uploaded

<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>diff</span>
<span style='color:#5faaff'>app</span>: `web_app` will be compiled
<span style='color:#5faaff'>server</span>: will be launched
<span style='color:#5faaff'>upload</span>: `web_app` will be uploaded
</pre>

{% end %}

{% card(id="visibility_desc") %}
Automation is not just about performant execution, but understanding the state of things, and what automation would alter.

Understanding is not "how *much* information can be output", but "how information is *best* presented".

Peace presents the highest value information at a glance, with easy options to go deeper.
{% end %}

</div>


## Adaptive Presentation

<div class="card_set">

{% card(id="adaptive_presentation") %}

<pre class="terminal">
<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>apply</span>  <span style='color:#7f7f7f'># interactive</span>
✅ <span style='color:#5faaff'>app   </span>▕<span style='background:#00af5f'>             </span>▏built in 4s
🔨 <span style='color:#5faaff'>server</span>▕<span style='background:#0087d7'>    </span><span style='background:#00005f'>         </span>▏el: 7s, eta: 9s
⏳ <span style='color:#5faaff'>upload</span>▕<span style='background:#000033'>             </span>▏deps: [server]

* To stop, press <span style='color:#bbbbbb'>Ctrl + C</span>
* For detail, in another terminal run:
  <span style='color:#5fff87'>envman</span> <span style='color:#bbbbbb'>progress --detail</span>
</pre>

<pre class="terminal">
<span style='color:#7f7f7f'># ci</span>
server | 1/5 |  0s | vm requested
app    | 1/1 |  4s | built
server | 2/5 |  7s | os booted
server | 3/5 | 10s | pkgs installed
</pre>

{% end %}

{% card(id="adaptive_presentation_desc") %}

<div id="adaptive_presentation_dot" class="static_size"></div>

Peace presents information and actions adaptively:

* Progress presented as bars when used interactively, and single line logs in CI.
* JSON when returned as a web response.
* HTML elements when rendered in a browser -- WASM / web application.

{% end %}

</div>


## Empirical Usability

<div class="card_set">

{% card(id="empirical_usability") %}

<pre class="terminal">
<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>apply</span>
✅ <span style='color:#5faaff'>app   </span>▕<span style='background:#00af5f'>             </span>▏built in 4s
⚠️ <span style='color:#5faaff'>server</span>▕<span style='background:#eecc00'>       </span><span style='background:#664400'>      </span>▏el: 33s, eta: 7s
⏳ <span style='color:#5faaff'>upload</span>▕<span style='background:#000033'>             </span>▏deps: [server]

* To stop, press <span style='color:#bbbbbb'>Ctrl + C</span>
* For detail, in another terminal run:
  <span style='color:#5fff87'>envman</span> <span style='color:#bbbbbb'>progress --detail</span>

<span style='color:#eecc00'>⚠️ warn:</span> <span style='color:#5faaff'>server</span> may have stalled.
  Last message:  <span style='color:#bbbbbb'>(9s ago)</span>
  <span style='color:#bbbbbb'>Running `sudo apt install -y sysstate` </span>
</pre>

{% end %}

{% card(id="empirical_usability_desc") %}

Peace automatically tracks execution metrics. This enables:

* **Proactive Outlier Detection:** Prompts based on outliers to historical averages.
* **Highest Impact Optimization:** Targeting efficiency improvements at the most frequent operations, or the slowest processes.

{% end %}

</div>


## Understandable Errors

<div class="card_set">

{% card(id="understandable_errors") %}

<pre class="terminal">
<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>apply</span>
✅ <span style='color:#5faaff'>app   </span>▕<span style='background:#00af5f'>             </span>▏built in 4s
❌ <span style='color:#5faaff'>server</span>▕<span style='background:#cc0000'>       </span><span style='background:#331111'>      </span>▏el: 33s, eta: 7s
🚫 <span style='color:#5faaff'>upload</span>▕<span style='background:#331111'>             </span>▏deps: [server]

Error: <span style='color:#e00'>server_package_install</span>

  <span style='color:#e00'>×</span> Server packages failed to install.
  <span style='color:#e00'>╰─▶</span> Unable to locate package sysstate
   ╭─[<span style='color:#7df'><b><u>server/config.yaml</u></b></span>:1:1]
 <span style='opacity:0.67'>1</span> │ packages:
 <span style='opacity:0.67'>2</span> │ - sysstate
   · <span style='color:#e0e'><b>  ───┬────</b></span>
   ·   <span style='color:#e0e'><b>   ╰── defined here</b></span>
   ╰────
   <span style='color:#7df'>Help:</span> Ensure spellings are correct
</pre>
{% end %}

{% card(id="understandable_errors_desc") %}

Peace presents errors in the much loved Rust compiler error format, through compatibility with [`miette`](https://github.com/zkat/miette) by design.

* The source parameter(s) that cause the error are presented to the user, reducing work to find it.
* Suggestions on how to recover help the user move forward.

People don't feel lost when what they do doesn't work; they feel lost when they don't have a way to move forward.

{% end %}

</div>


## Execution History

<div class="card_set">

{% card(id="execution_history") %}

<pre class="terminal">
<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>log</span>
profile: <span style='color:#5fff87'>development</span>
<span style='color:#eeeeee'>2023-01-25+1300</span> <span style='color:#bbbbbb'>(2 days ago)</span>
* ✅ <span style='color:#5faaff'>8</span> <span style='color:#bbbbbb'>15:28:20</span> envman clean
* ✅ <span style='color:#5faaff'>7</span> <span style='color:#bbbbbb'>14:56:32</span> envman apply
* ✅ <span style='color:#5faaff'>6</span> <span style='color:#bbbbbb'>14:48:22</span> envman apply
* ❌ <span style='color:#5faaff'>5</span> <span style='color:#bbbbbb'>14:47:19</span> envman apply
* ✅ <span style='color:#5faaff'>4</span> <span style='color:#bbbbbb'>14:31:48</span> envman apply
<span style='color:#eeeeee'>2023-01-24+1300</span> <span style='color:#bbbbbb'>(3 days ago)</span>
* ✅ <span style='color:#5faaff'>3</span> <span style='color:#bbbbbb'>17:05:17</span> envman clean
* ✅ <span style='color:#5faaff'>2</span> <span style='color:#bbbbbb'>16:32:08</span> envman apply
* ✅ <span style='color:#5faaff'>1</span> <span style='color:#bbbbbb'>16:29:59</span> envman apply
</pre>

{% end %}

{% card(id="execution_history_desc") %}
<pre class="terminal">
<span class='shell'>&gt; </span><span class='cmd'>envman</span> <span class='arg'>show</span>
<span style='color:#bbbbbb'>last execution:</span> <span style='color:#5faaff'>8</span>
<span style='color:#bbbbbb'>command:</span> envman clean
<span style='color:#eeeeee'>2023-01-25T15:28:20+1300</span> <span style='color:#bbbbbb'>(2 days ago)</span>

<span style='color:#5faaff'>app</span>:    up-to-date    to not-exists
<span style='color:#5faaff'>server</span>: `abc-103abfe` to None
<span style='color:#5faaff'>upload</span>: n/a
</pre>

Peace stores execution summaries in its structured form, making it easy to review what was done, or analyzed with a different presentation.
{% end %}

</div>


## Constrained Code

<div class="card_set">

{% card(id="constrained_code") %}

```rust
trait ItemSpec {
    type State:  /* .. */;
    type StateDiff:  /* .. */;
    async fn state_current(&self, ..) -> _;
    async fn apply_exec(&self, ..) -> _;
    /* .. */
}

impl Presentable for ServerState {
    fn present(&self, presenter: &Presenter) {
        presenter
            .name(self.server_name)
            .desc(self.server_status)
    }
}
```

{% end %}

{% card(id="constrained_code_desc") %}

Compile time constraints <small>(with all features)</small> means correctness, safety, and ease-of-use by design:

* For every item creation, there is a clean up.
* Every command has a target state, so *multiple executions is not dangerous*.
* Item states must be:
    - **Fetchable:** For idempotence, this allows automation to skip to where it stopped.
    - **Presentable:** In short for comprehension, in detail for informed decisions.
* Constraints enable Peace to provide common flows, and adapt information for each target.

*⛓️ It is these constraints that give us freedom*

{% end %}

</div>


## Incremental Maturity

<div class="card_set">

{% card(id="constrained_code") %}
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
 async fn apply_exec(
     state_current: Self::State,
     state_desired: Self::State,
+    state_diff: Self::StateDiff,
 ) -> Result<(), E>
```

🦀 *Compilation errors* show the code that needs updating, you don't have to work to find out.

{% end %}

</div>


# Building Blocks / Concepts

## Item Specification

<div class="card_set">
{{ card(id="item_spec_dot", class="static_size") }}
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

<div class="card_set">
{{ card(id="item_spec_graph_dot", class="static_size") }}
{% card(id="item_spec_graph_desc") %}

A [directed graph](https://en.wikipedia.org/wiki/Directed_graph) of **Item Spec**s. Edges indicate forward execution order.

Functions can be executed in parallel when their predecessors' executions are complete.

{% end %}
</div>


## Flows

<div class="card_set">

{{ card(id="flows_dot", class="static_size") }}

{% card(id="flows_desc") %}

A flow is a process that fulfills a business question or intent. Within a flow, the appropriate Item Specification *function*(s) are invoked for each *item*.

Examples:

* Discover and show the state of all items.
* Show what would change.
* Execute the automation.

Advanced flows may invert execution direction:

* `clean` cleans up items in reverse order.
* `apply` on a *subset* of items may forward alter some items, and clean others.

{% end %}

</div>


# The Peace Disadvantage

*Reasons you would **not** use Peace*

1. Higher automation maturity levels implies higher cost of development:

    Low maturity automation can evolve faster as it has fewer constraints. This is sustainable when:

    - The number of users is not high.
    - The cost of higher resilience and usability is not justified.

2. Rust is not as common a skillset as other languages, and requires substantial time to learn.


# Inspirations

## People

Through working with many people across different roles, it is apparent that a lot of issues when working with automation can be solved through clearer and more understandable communication &ndash; a dimension that deserves more attention in automation tooling.


## Git

Git allows one to show the state of local and remote repositories, diff them, before committing and pushing the changes. Pushes are atomic and idempotent, meaning running the same command multiple times, whether or not the operation succeeded prior, has the same result.

The idea spawned from this is, instead of file content as the state, and patches being the diff, why not something &ndash; *anything* &ndash; user defined?


## Rust

Rust's commitment to helping people learn, with clear explanations and examples, demonstrates that taking care of people is a priority that should not be left behind amidst technical advancement.


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
            fontname  = "helvetica"
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
