+++
title = "Had I Had Time"
description = "⏲️ Had I Had Time"
date = "2023-06-29"

[taxonomies]
categories = ["development"]
tags = ["concepts"]
+++

> Had I had time, what would perfect automation look like?

# So Far

## For Users

From the book of "don't overwhelm me while I'm learning" (or, *ever*), the user experience has been refined to be understandable and safe.


### Solid Foundations

Automation software built using the Peace framework allows users to:

* See what would happen, *before* making it happen.
* Press "go" multiple times, without adverse effects.

<video controls width="100%"><source src="envman_basic_demo.mp4" type="video/mp4" /></video>


### Changes Of Mind

There is some resilience to changes of mind and interruptions, and *you are not punished for changing your mind*:

<video controls width="100%"><source src="envman_interruption_demo.mp4" type="video/mp4" /></video>

<sub>ℹ️ Also, that state discovery step after interruption won't be necessary when [peace#141](https://github.com/azriel91/peace/issues/141) is implemented</sub>


### Profile Support

Peace is designed to support for working with multiple environments; though this part is still early days:

<video controls width="100%"><source src="envman_profile_demo.mp4" type="video/mp4" /></video>


## For Developers

### Write Code, Not Strings

Type safety, compile time support, and code assist enables:

* Easy discovery of what parameters are available &ndash; no more searching and pasting large chunks of YAML / JSON.
* Errors at compile time &ndash; no longer waste time deploying to discover *minor* issues.
* Writing code to pass values between items, not string templates.

Real code from <a href="https://github.com/azriel91/peace/blob/0.0.11/examples/envman/src/flows/env_deploy_flow.rs#L134-L148">`env_deploy_flow.rs@0.0.11`</a>:

```rust
let iam_role_params_spec = IamRoleParams::<WebApp>::field_wise_spec()
    .with_name(iam_role_name)
    .with_path(path.clone())
    .with_managed_policy_arn_from_map(|iam_policy_state: &IamPolicyState| {
        if let IamPolicyState::Some {
            policy_id_arn_version: Generated::Value(policy_id_arn_version),
            ..
        } = iam_policy_state
        {
            Some(policy_id_arn_version.arn().to_string())
        } else {
            None
        }
    })
    .build();
```



### No Minor Annoyances

The little things are taken care of:

* Sorted values for clear and understandable diffs.
* Sensible directory structure for separate and common values within a namespace.

```diff
❯ diff -u .peace/envman/demo_1/env_deploy/{states_goal.yaml,states_current.yaml}
 iam_role: !Some
   name: demo_1
   path: /
-  role_id_and_arn: !Tbd null
+  role_id_and_arn: !Value
+    id: AROASBLNZJUXOTGGPP7PX
+    arn: arn:aws:iam::140353555758:role/demo_1
   ..
```

```
repo/.peace/envman/demo_1/env_deploy
     ^      ^      ^      ^
     |      |      |      '--------- flow: cleanly separate information for different
     |      |      |                 workflows, e.g. "deploy" vs "load data"
     |      |      |
     |      |      '----- profile: logically separates environments
     |      |
     |      '----- tool name: allows multiple tools to be run in the same repo
     |
     '----- `.peace` data directory, like `.git`
```

*Take care of the little things, that make the little difference, that make the big difference.*


# Up Ahead

There are three dimensions that I'd like to develop Peace further:

## Production Readiness

<div id="production_readiness_dot"></div>

The lifetime of an environment should be able to exist before, and live beyond, a version of the automation software that manages it.

For the Peace framework to be a sensible choice to build a production tool, the framework and items must be able to handle evolving data and logic. Namely, handling cases like:

* Terminate these servers, recreate the VPC with resized CIDR blocks, then launch new servers in the new VPC.
* This infrastructure was created manually, now it should be automatically managed, and migrated to use new naming conventions.

The [Peace: Production Readiness] GitHub project captures this work. See [Flow Versioning](https://peace.mk/book/technical_concepts/flow_versioning.html) for my technical thinking out loud notes.


## User Interface

<div id="user_interface_dot"></div>

Two common methods to access automation are the command line, and the web browser.

Peace should provide support for:

* Invoking commands to interact with the environment.
* Presenting common data &ndash; usually `States`, but also `ParamsSpecs`, `Params`, errors, and progress.

Some of the CLI presentation logic currently lives in the `envman` example, and should be moved into `peace`, and the web presentation logic is currently "bundle [`leptos`](https://github.com/leptos-rs/leptos) with the command line app and render the flow graph".

The [Peace: UI] GitHub project captures the start of this work. For the web UI part, see <a href="https://github.com/azriel91/peace/blob/0.0.11/DEVELOPMENT.md#web-development">`DEVELOPMENT.md#web-development`</a>.


## API Refinement

<div id="api_refinement_dot"></div>

There is some API that doesn't prevent developers from making mistakes or having mild annoyances through natural usage of the API.

The API should be refined to avoid setting up such situations, and implement additional functionality that makes it easier to work with information across profiles or across flows.

The [Peace: API Refinement and Features] GitHub project captures this work.

---

Had I had time, developing these would be excellent.

But I don't &ndash; I'm off to work!  
🏃🏽‍♂️💨

[Peace: Production Readiness]: https://github.com/users/azriel91/projects/1
[Peace: UI]: https://github.com/users/azriel91/projects/2
[Peace: API Refinement and Features]: https://github.com/users/azriel91/projects/3

<script type="text/javascript">
// Hack to set the playback rate for videos.
// You can't set it in the HTML element.
document.querySelectorAll("video").forEach(v => { v.playbackRate = 2.0; });
</script>

<script type="module">
    import { Graphviz } from "https://cdn.jsdelivr.net/npm/@hpcc-js/wasm/dist/graphviz.js";

    const graphviz = await Graphviz.load();
    const production_readiness_dot = `digraph G {
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

        subgraph cluster_env_v1 {
            label = "env_v1"
            labelloc = "bottom"

            env_v1_a [label = ""]
            env_v1_b [label = ""]
            env_v1_c [label = ""]
            env_v1_d [label = ""]
            env_v1_e [label = "", style = "invis"]

            env_v1_a -> env_v1_b
            env_v1_a -> env_v1_c
            env_v1_b -> env_v1_d
            env_v1_b -> env_v1_e [style = "invis"]
            env_v1_c -> env_v1_e [style = "invis"]
        }

        subgraph cluster_env_v2 {
            label = "env_v2"
            labelloc = "bottom"

            env_v2_a [label = ""]
            env_v2_b [label = ""]
            env_v2_c [label = ""]
            env_v2_d [label = ""]
            env_v2_e [label = "", color="#449966", fillcolor="#88cc88"]

            env_v2_a -> env_v2_b
            env_v2_a -> env_v2_c
            env_v2_b -> env_v2_d
            env_v2_b -> env_v2_e
            env_v2_c -> env_v2_e
        }

        subgraph cluster_env_v3 {
            label = "env_v3"
            labelloc = "bottom"

            env_v3_a [label = ""]
            env_v3_b [label = "", color="#994444", fillcolor="#cc8888"]
            env_v3_c [label = ""]
            env_v3_d [label = "", color="#994444", fillcolor="#88cc88", style="dashed,filled"]
            env_v3_e [label = "", color="#994444", fillcolor="#88cc88", style="dashed,filled"]

            env_v3_a -> env_v3_b
            env_v3_a -> env_v3_c
            env_v3_b -> env_v3_d
            env_v3_b -> env_v3_e
            env_v3_c -> env_v3_e
        }

        env_v1_d -> env_v2_a [style = "invis"]
        env_v1_e -> env_v2_a [style = "invis"]

        env_v2_d -> env_v3_a [style = "invis"]
        env_v2_e -> env_v3_a [style = "invis"]
    }`;
    document.getElementById("production_readiness_dot").innerHTML = graphviz.layout(production_readiness_dot, "svg", "dot");

    const user_interface_dot = `digraph G {
        graph [
            margin    = 0.0
            penwidth  = 0
            nodesep   = 0.0
            ranksep   = 0.02
            bgcolor   = "transparent"
            fontname  = "helvetica"
            fontcolor = "#7f7f7f"
            rankdir   = LR
            splines   = line
        ]
        node [
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
            penwidth  = 3
        ]
        edge [
            arrowsize = 0.7
            color     = "#7f7f7f"
            fontcolor = "#7f7f7f"
            penwidth  = 3
        ]

        subgraph cluster_a {
            node [color="#449966" fillcolor="#99dd99"]
            a [label = <<b>a</b>>]
            a_text [shape="plain" style="" fontcolor="#7f7f7f" label = <<table border="0" cellborder="0" cellpadding="0"><tr>
                <td><font point-size="15">📥</font></td>
                <td balign="left">file<br/>download</td>
            </tr></table>>]
        }

        subgraph cluster_b {
            node [color="#446699" fillcolor="#99aaee"]
            b [label = <<b>b</b>> class="item_b_in_progress" style="dashed,filled"]
            b_text [shape="plain" style="" fontcolor="#7f7f7f" label = <<table border="0" cellborder="0" cellpadding="0"><tr>
                <td><font point-size="15">🪣</font></td>
                <td balign="left">s3<br/>bucket</td>
            </tr></table>>]
        }

        subgraph cluster_c {
            c [label = <<b>c</b>>]
            c_text [shape="plain" style="" fontcolor="#7f7f7f" label = <<table border="0" cellborder="0" cellpadding="0"><tr>
                <td><font point-size="15">📤</font></td>
                <td balign="left">s3<br/>object</td>
            </tr></table>>]
        }

        a -> c [minlen = 9, color="#66aa66"]
        b -> c [minlen = 9, color="#6688bb"]
    }`;
    document.getElementById("user_interface_dot").innerHTML = graphviz.layout(user_interface_dot, "svg", "dot");
</script>

<style type="text/css">
.item_b_in_progress ellipse {
    animation: 10.0s linear forwards 0.0s infinite item_b_in_progress;
}
@keyframes item_b_in_progress {
    0% {
        transform-origin: 42.16px -56px;
/*        transform-origin: attr(cx px) attr(cy px);*/
        transform: rotate(0deg);
    }
    100% {
        transform-origin: 42.16px -56px;
/*        transform-origin: attr(cx px) attr(cy px);*/
        transform: rotate(360deg);
    }
}
</style>
