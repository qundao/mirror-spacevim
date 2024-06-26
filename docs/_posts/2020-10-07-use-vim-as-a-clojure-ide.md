---
title: "Use Vim as a Clojure IDE"
categories: [tutorials, blog]
image: https://img.spacevim.org/95338841-f07a1e00-08e5-11eb-9e1b-6dbc5c4ad7de.png
description: "A general guide for using SpaceVim as Clojure IDE, including layer configuration, requiems installation and usage."
type: article
comments: true
commentsID: "Use Vim as a Clojure IDE"
language: Clojure
---

# [Blogs](../blog/) >> Use Vim as a Clojure IDE

This is a general guide for using SpaceVim as a Clojure IDE, including layer configuration and usage. 
Each of the following sections will be covered:

![clojure-ide](https://img.spacevim.org/95338841-f07a1e00-08e5-11eb-9e1b-6dbc5c4ad7de.png)

<!-- vim-markdown-toc GFM -->

- [Enable language layer](#enable-language-layer)
- [code completion](#code-completion)
- [Language server](#language-server)
  - [neovim(`>=0.5.0`)](#neovim050)
  - [vim or neovim(`<0.5.0`)](#vim-or-neovim050)
- [alternate file jumping](#alternate-file-jumping)
- [code running](#code-running)
- [REPL support](#repl-support)
- [code format](#code-format)
- [Tasks manage](#tasks-manage)

<!-- vim-markdown-toc -->

### Enable language layer

`lang#clojure` layer provides clojure language specific features for SpaceVim.
This layer is not enabled by default. To write clojure language,
you need to enable the `lang#clojure` layer.
Press `SPC f v d` to open SpaceVim configuration file, and add following configuration:

```toml
[[layers]]
  name = 'lang#clojure'
```

for more info, you can read the [lang#clojure](../layers/lang/clojure/) layer documentation.

### code completion

By default the autocomplete layer has been enabled, so after loading `lang#clojure` layer, the code completion
for clojure language should work well.

### Language server

There are two ways to configure the clojure language server protocol.

#### neovim(`>=0.5.0`)

If you are using `nvim(>=0.5.0)`. You need to use `enabled_clients` to specific the language servers.
for example:

```toml
[[layers]]
    name = 'lsp'
    enabled_clients = ['clojure_lsp']
```

To override the command of client, you may need to use `override_client_cmds` option:

```toml
[[layers]]
  name = "lsp"
  enabled_clients = ['clojure_lsp']
  [layers.override_client_cmds]
    clojure_lsp = ['clojure-lsp']
```

You need to install [`clojure-lsp`](https://clojure-lsp.io/installation/).

```
scoop install clojure-lsp
```

#### vim or neovim(`<0.5.0`)

To enable language server protocol support, you may need to enable lsp layer.

```toml
[[layers]]
  name = "lsp"
  filetypes = [
    "clojure"
  ]
  [layers.override_cmd]
    clojure = ['clojure-lsp']
```

### alternate file jumping

To manage the alternate file for a project, you may need to create a `.project_alt.json` file in the root of your
project.

for example, add following content into the `.project_alt.json` file:

```json
{
  "src/*.clj": {"alternate": "test/{}.clj"},
  "test/*.clj": {"alternate": "src/{}.clj"}
}
```

with this configuration, you can jump between the source code and test file via command `:A`


### code running

The default code running key binding is `SPC l r`. It will run `clojure -M current_file` asynchronously.
And the stdout will be shown on a runner buffer.

![clojure-runner](https://img.spacevim.org/95334765-1a7d1180-08e1-11eb-8c78-9a87d61d3d63.png)

### REPL support

![clojure-repl](https://img.spacevim.org/95341519-f1f91580-08e8-11eb-9280-04f89875dc78.png)

`lang#clojure` layer provides key bindings for REPL support of clojure language.
You can Start a `clojure` inferior REPL process with `SPC l s i`. After REPL process started,
you can send code to `clojure` process via key bindings:

| Key Bindings | Descriptions                                     |
| ------------ | ------------------------------------------------ |
| `SPC l s b`  | send buffer and keep code buffer focused         |
| `SPC l s l`  | send line and keep code buffer focused           |
| `SPC l s s`  | send selection text and keep code buffer focused |


### code format

The format layer use neoformat as default tool to format code, it will run `cljfmt` on current file.
And the default key binding is `SPC b f`.

```toml
[[layers]]
  name = "format"
```

### Tasks manage

SpaceVim will detect [`lein`](https://leiningen.org/) project tasks automatically. If there is `project.clj` file in the root directory
of your project. following tasks will be detected:

![taskmanager](https://img.spacevim.org/95338987-1a334500-08e6-11eb-80c4-ad811095d8c8.png)

To select a task to run, use key binding `SPC p t r`, you can also use `SPC p t l` to list all the tasks
in the tasks manager window.
