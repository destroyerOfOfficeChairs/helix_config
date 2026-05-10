# Helix Editor Configuration

This repository contains my configuration files for the Helix text editor. It is optimized for a keyboard-driven workflow, specifically tuned for writing, documentation, worldbuilding using Typst, and web development using Tailwind CSS.

## Overview

This setup enhances the default Helix experience with a few quality-of-life UI adjustments and a specialized language server setup for prose, markup, and utility-first CSS. 

### Key Features

* **Intelligent Spellchecking:** Integrates the Rust-based `harper-ls` language server for fast, syntax-aware grammar and spellchecking.
* **Dual Language Servers (Typst):** Configures Helix to run `tinymist` and `harper-ls` simultaneously for `.typ` files. This ensures standard compiler errors and autocompletion remain active alongside the grammar linting.
* **Dual Language Servers (Web):** Runs `tailwindcss-ls` alongside `vscode-html-language-server` for intelligent HTML tag completion and live CSS compilation in hover menus.
* **Document Formatting:** Enforces an 80-character text width with a visual ruler and soft-wrapping for plain text and Typst files to keep documents readable.
* **UI & Keybinds:** Includes a custom, decluttered statusline and maps `jj` to quickly exit insert mode.

## Installation & Setup

When setting up a new machine, follow these steps to restore this configuration.

### 1. Install Dependencies

You will need the Helix editor, language servers for Typst, and Node.js for the web tools. 

**For Prose & Typst:**
Install Harper and Tinymist via Cargo:
```bash
cargo install harper-ls
# Note: Because the main crates.io package is a library, install the CLI binary directly from Git.
cargo install --git [https://github.com/Myriad-Dreamin/tinymist](https://github.com/Myriad-Dreamin/tinymist) --locked tinymist-cli
```

**For HTML & Tailwind CSS v4:**
*Requirement: Tailwind's Oxide engine requires Node.js v20 or higher. Use `nvm` to install Node v22+ if your system repositories are outdated.*

Install the global language servers via npm:
```bash
npm install -g @tailwindcss/language-server
npm install -g vscode-langservers-extracted
```

### 2. Link Configuration Files

Helix looks for configuration files in `~/.config/helix/`. Instead of copying the files directly, create symbolic links from this repository so that future edits are automatically tracked by Git.

Run the following commands to remove the default static files (if they exist) and create the symlinks:
```bash
# Ensure the Helix config directory exists
mkdir -p ~/.config/helix

# Remove any existing default files
rm -f ~/.config/helix/languages.toml
rm -f ~/.config/helix/config.toml

# Create symlinks pointing to this repository
ln -s ~/Documents/repos/helix_config/languages.toml ~/.config/helix/languages.toml
ln -s ~/Documents/repos/helix_config/config.toml ~/.config/helix/config.toml
```

### 3. Verify Setup

To ensure Helix recognizes the new configuration and language servers, open your terminal and run:

```bash
hx --health
```

Check that `tinymist`, `harper-ls`, `tailwindcss-ls`, and `vscode-html-language-server` are listed and active. You can also open a file in Helix and type `:log-open` to view the server startup logs.

---

### Helix Multi-LSP Quirks
* **Hover Documentation Priority:** Helix only displays the hover menu (`Space k`) for the *first* server that responds. In `languages.toml`, `tailwindcss-ls` is prioritized over `vscode-html-language-server`. Pressing `Space k` on utility classes will show compiled CSS, but pressing it on standard HTML tags will likely return nothing.
* **Copying from Hover Menus:** To copy text from the `Space k` popup, hold `Shift` while highlighting with the mouse to temporarily bypass Helix's mouse capture.

### Managing the Harper Dictionary
Harper maintains local dictionaries to prevent custom worldbuilding terms, character names, or technical jargon from being flagged.

When hovering over a flagged word in Helix, press `Space a` to open the Code Actions menu and add the word to your dictionary.

* **Global User Dictionary:** Stored at `~/.config/harper-ls/dictionary.txt`
* **Workspace Dictionary:** Create a `.harper-dictionary.txt` file at the root of a specific project
