# 🕊️ Peace Website &ndash; <https://peace.mk>

[![Website CI](https://github.com/azriel91/peace_website/actions/workflows/publish.yml/badge.svg)](https://github.com/azriel91/peace_website/actions/workflows/publish.yml)

Website for the [Peace automation framework](https://github.com/azriel91/peace).

## Building

1. Install [`zola`](https://www.getzola.org/).

    ```bash
    cargo install zola
    ```

2. Start the website:

    ```bash
    zola serve
    ```

### Technical Documentation Book

The **Book** part of the site is maintained in the [`peace`](https://github.com/azriel91/peace) repository under `docs`.

The following steps build the book and have it served by `zola`.

1. Install book dependencies:

    ```bash
    sudo apt install graphviz
    cargo install mdbook
    cargo install mdbook-graphviz
    ```

2. Clone the `peace` repository beside the `peace_website` repository:

    ```bash
    # beside peace_website
    git clone git@github.com:azriel91/peace
    ```

3. Build the WASM examples:

    ```bash
    cd peace
    for example in $(ls examples)
    do wasm-pack build \
      --target web \
      --out-dir "../../../peace_website/static/book/examples/pkg" \
      --release \
      "examples/${example}" \
      --features 'error_reporting output_colorized output_json'
    done
    cd -
    ```

4. Build the book:

    ```bash
    cd peace_website
    mdbook build ../peace/doc -d ../../peace_website/static/book
    ```

    The web server run by `zola serve` will automatically serve the book at:

    <http://127.0.0.1:1111/book/introduction.html>
