+++
title = "Conceptual Introduction"
description = "Conceptual Introduction"
date = "2023-04-03"

[taxonomies]
categories = ["development"]
tags = ["concepts"]
+++


How does the Peace framework shape automation to be resilient and provide a good user experience?


# Automation Model

> The following is a simplified model of what is used in the Peace framework, and is for teaching, not for accuracy.

1. Imagine we have the following automation steps:

    1. Compile an application.
    2. Launch a server.
    3. Upload the application to the server.

    <div id="automation_process_dot"></div>

2. Each of these steps is a write function, connected together:

    ```rust
    fn app_compile()   -> _ { sh!("cargo build"); .. }
    fn srvr_launch()   -> _ { sh!("ec2 run-instances .."); .. }
    fn file_upload(..) -> _ { sh!("scp {src} user@{ip}:{dest}"); .. }

    // Invoke the functions
    let path = app_compile();
    let ip   = server_launch();
    let _    = file_upload(path, dest, ip);
    ```

3. Instead of only having a write function for each step, we define read functions:

    The following show 3 functions for each step:

    1. Current state.
    2. Desired state.
    3. Logic to change the current state into the desired state.

    ```rust
    // Application
    fn app_last_ts(..) -> Time { sh!("stat -c '%Y' target/debug/app"); .. }
    fn app_src_ts(..)  -> Time { sh!("stat -c '%Y' **/*.rs"); .. }
    fn app_compile(..) -> _    { sh!("cargo build"); .. }

    // Server
    fn srvr_status(..) -> Srvr { sh!("ec2 describe-instances .."); .. }
    fn srvr_spec(..)   -> Srvr { sh!("cat server.yaml"); .. }
    fn srvr_launch(..) -> _    { sh!("ec2 run-instances .."); .. }

    // File Upload
    fn file_dest(..)   -> Md5Sum { sh!("ssh user@{ip} -C 'md5sum {dest}'"); .. }
    fn file_src(..)    -> Md5Sum { sh!("md5sum {src}"); .. }
    fn file_upload(..) -> _      { sh!("scp {src} user@{ip}:{dest}"); .. }
    ```

4. With the current and desired states, we can calculate a diff, and whether we need to do work:

    ```rust
    // Application
    fn app_ts_diff(Time, Time)      -> TimeDiff { .. }
    fn app_compile_needed(TimeDiff) -> bool { .. }

    // Server
    fn srvr_diff(Srvr, Srvr)        -> SrvrDiff { .. }
    fn srvr_launch_needed(SrvrDiff) -> bool { .. }

    // File Upload
    fn file_diff(Md5Sum, Md5Sum)      -> Md5SumDiff { .. }
    fn file_upload_needed(Md5SumDiff) -> bool       { .. }
    ```

5. Put these functions and data types into a trait:

    ```rust
    // Specific functions
    fn file_dest(..)                  -> Md5Sum { sh!("ssh user@{ip} -C 'md5sum {dest}'"); .. }
    fn file_src(..)                   -> Md5Sum { sh!("md5sum {src}"); .. }
    fn file_diff(Md5Sum, Md5Sum)      -> Md5SumDiff { .. }
    fn file_upload_needed(Md5SumDiff) -> bool       { .. }
    fn file_upload(..)                -> _      { sh!("scp {src} user@{ip}:{dest}"); .. }

    // Genericized grouping of step functions, and data.
    trait ItemSpec {
        type State;
        type StateDiff;

        fn state_current() -> Self::State;
        fn state_desired() -> Self::State;
        fn state_diff(Self::State, Self::State) -> Self::StateDiff;
        fn check(Self::StateDiff) -> bool;
        fn apply(Self::State, Self::State, Self::StateDiff);
    }
    ```

6. Peace groups all the items into a graph:

    <div id="item_spec_graph_dot"></div>


7. Peace provides *commands* to run different combinations of each item's functions:

    <div id="flows_dot"></div>

    - `StatesDiscoverCmd`: Runs `state_current` for all items.
    - `ApplyCmd`: Given a target state, runs the following for all items:
        1. `state_current`
        2. `state_desired`
        3. `diff`
        4. `check`
        5. `apply`


# Ending Note

Peace implements other concepts to provide resilience and good user experience, and these will be written in future posts.

Peace is still evolving, and is not ready for general adoption.


<script type="module">
    import { Graphviz } from "https://cdn.jsdelivr.net/npm/@hpcc-js/wasm/dist/graphviz.js";

    const graphviz = await Graphviz.load();
    const automation_process_dot = `digraph G {
        graph [
            penwidth  = 0
            nodesep   = 0.0
            ranksep   = 0.8
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
            a [label = <<b>a</b>>]
            a_text [shape="plain" style="none" fontcolor="#7f7f7f" label = <app<br/>compile>]
        }
        subgraph cluster_b {
            margin = 0

            b [label = <<b>b</b>>]
            b_text [shape="plain" style="none" fontcolor="#7f7f7f" label = <server<br/>launch>]
        }
        subgraph cluster_c {
            c [label = <<b>c</b>>]
            c_text [shape="plain" style="none" fontcolor="#7f7f7f" label = <file<br/>upload>]
        }

        a -> b
        b -> c
    }`;
    document.getElementById("automation_process_dot").innerHTML = graphviz.layout(automation_process_dot, "svg", "dot");

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
</script>
