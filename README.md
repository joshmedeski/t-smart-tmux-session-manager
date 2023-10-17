<a href="https://www.joshmedeski.com/posts/smart-tmux-sessions-with-zoxide-and-fzf/" target="_blank">

![thumbnail](https://github.com/joshmedeski/t-smart-tmux-session-manager/blob/main/smart-tmux-sessions-with-zoxide-and-fzf.jpeg?raw=true)

</a>

# t - the smart tmux session manager

tmux is a powerful tool, but dealing with sessions can be painful. This script makes it easy to create and switch tmux sessions:

## Prerequisites

- [tmux](https://github.com/tmux/tmux) (>= 3.2)
- [tpm](https://github.com/tmux-plugins/tpm)
- [bash](https://www.gnu.org/software/bash/) (>= 4.0)
- [zoxide](https://github.com/ajeetdsouza/zoxide)
- [fzf](https://github.com/junegunn/fzf) (>=0.35.0)

**Note:** Macs have bash v3 preinstalled, you'll need v4 or later for this script. I recommend installing these prerequisites from homebrew to get the latest versions.

```
brew install tmux bash zoxide fzf
```

Use the package manager of your OS if you are not on macOS.

## How to install

### 1. Install tpm plugin

Add the following line to your `tmux.conf` file:

```sh
set -g @plugin 'joshmedeski/t-smart-tmux-session-manager'
```

**Note:** tpm recommends you list your plugins and then run tpm at the very bottom of your `tmux.conf`.

Then, run `<prefix>I` to install the plugin.

### 2. Add to path

To use the `t` script from anywhere, select your shell environment and follow the instructions.

**Note:** you'll need to check the path of your tpm plugins. It may be `~/.tmux/plugins` or `~/.config/tmux/plugins` depending on where your `tmux.conf` is located.

<details>
<summary>zsh or bash</summary>

Add the following line to `~/.bashrc` for bash, and `~/.zprofile` for zsh.

```sh
# ~/.tmux/plugins
export PATH=$HOME/.tmux/plugins/t-smart-tmux-session-manager/bin:$PATH
```

```sh
# ~/.config/tmux/plugins
export PATH=$HOME/.config/tmux/plugins/t-smart-tmux-session-manager/bin:$PATH
```

</details>

<details>
<summary>fish</summary>

Add the following line to `~/.config/fish/conf.d/t.fish`

```sh
# ~/.tmux/plugins
fish_add_path $HOME/.tmux/plugins/t-smart-tmux-session-manager/bin
```

```sh
# ~/.config/tmux/plugins
fish_add_path $HOME/.config/tmux/plugins/t-smart-tmux-session-manager/bin
```

</details>

### 3. Recommended tmux settings

I recommend you add these settings to your `tmux.conf` to have a better experience with this plugin.

```sh
bind-key x kill-pane # skip "kill-pane 1? (y/n)" prompt
set -g detach-on-destroy off  # don't exit from tmux when closing a session
```

### 4. Customize Prompt (optional)

If your terminal supports [Nerd Font symbols](https://www.nerdfonts.com/), you can customize your prompt.

```sh
set -g @t-fzf-prompt '  '
```

Or you can replace the prompt with anything you'd like.

## How to use

By default, this plugin is bound to `<prefix>+T` which triggers a fzf-tmux popup that display zoxide results. Type the result you want and when you hit enter it will create a tmux session and connect to it or, if the sessions already exists, switch to it.

If you are not in tmux, you can simply run `t` to start the interactive script, or call `t {name}` to jump directly to a session of your choosing.

You can run `t --help` or `t -h` to see the help menu.

## How to customize

There are many ways you can customize this script to fit your workflow. I am also open to pull requests if you have ideas for improvements. Customizations are stored as shell variables, so they can be accessed with or without tmux running. Here are the available variables:

- `T_FZF_FIND_BINDING`: overwrites the fzf `ctrl-f` key binding, typically used for finding a directory that's not listed in zoxide.
- `T_FZF_PROMPT`: overwrites the fzf prompt symbol
- `T_SESSION_USE_GIT_ROOT`: detects the git root of a chosen directory when generating a tmux sessions name
- `T_SESSION_NAME_INCLUDE_PARENT`: includes the parent directory in the tmux session name
- `T_FZF_BORDER_LABEL`: overwrites the fzf popup border label
- `T_FZF_DEFAULT_RESULTS`: overwrites the default fzf results, can be "sessions" or "zoxide" (defaults to sessions and zoxide)

<details>
<summary>zsh or bash</summary>

Add the following line to `~/.zshrc` for zsh, and `.bashrc` for bash.

```sh
# use fd and search two levels deep from home
export T_FZF_FIND_BINDING = 'ctrl-f:change-prompt(  )+reload(fd -H -d 2 -t d -E .Trash . ~)'
export T_FZF_PROMPT = '🧠'
export T_SESSION_USE_GIT_ROOT = true
export T_SESSION_NAME_INCLUDE_PARENT = true # will fallback to parent if git root is not found
export T_FZF_BORDER_LABEL = 'the ultimate tmux tool'
export T_FZF_DEFAULT_RESULTS = 'sessions'
```

</details>

<details>
<summary>fish</summary>

Add the following line to `~/.config/fish/conf.d/t.fish`

```fish
set -Ux T_FZF_FIND_BINDING 'ctrl-f:change-prompt(  )+reload(fd -H -d 2 -t d -E .Trash . ~)'
set -Ux T_FZF_PROMPT '🧠'
set -Ux T_SESSION_USE_GIT_ROOT true
set -Ux T_SESSION_NAME_INCLUDE_PARENT true
set -Ux T_FZF_BORDER_LABEL 'the ultimate tmux tool'
set -Ux T_FZF_DEFAULT_RESULTS 'sessions'
```

</details>

**Note:** `T_SESSION_USE_GIT_ROOT` and `T_SESSION_NAME_INCLUDE_PARENT` are in conflict right now, so I recommend choosing one or the other.

### Custom fzf-tmux keybinding

By default, the `t` popup is bound to `<prefix>T`. In order to overwrite your own custom key binding, add setting the `@t-bind` varaible to your `tmux.conf`:

```sh
set -g @t-bind "K"
```

You can unbind the default by using `none`.

```sh
set -g @t-bind "none" # unbind default
```

### Change default fzf results

By default, t will display tmux sessions and zoxide results by default. You can change this by setting `@t-fzf-default-results` variable to your `tmux.conf`:

```sh
set -g @t-fzf-default-results 'sessions' # show tmux sessions by default
```

```sh
set -g @t-fzf-default-results 'zoxide' # show zoxide results by default
```

### Custom find command

By default, the find key binding (`^f`) will run a simple `find` command to search for directories in and around your home directory.

```sh
find ~ -maxdepth 3 -type d
```

You can customize this command by setting `@t-find-binding` variable to your `tmux.conf`:

In this example, I'm setting the prompt with a custom [Nerd Font icon](https://www.nerdfonts.com/) and using [fd](https://github.com/sharkdp/fd) to search directories (including hidden ones) up to two levels deep from my home directory.

```sh
set -g @t-fzf-find-binding 'ctrl-f:change-prompt(  )+reload(fd -H -d 2 -t d . ~)'
```

Run `man fzf` to learn more about how to customize key bindings with fzf.

### FZF_TMUX_OPTS

If you want to overwrite the fzf-tmux options, you can set the `FZF_TMUX_OPTS` variable in your shell environment.

```bash
# ~/.bashrc or ~/.zshrc
export FZF_TMUX_OPTS="-p 55%,60%"
```

```fish
# ~/.config/fish/config.fish
set -Ux FZF_TMUX_OPTS "-p 55%,60%"
```

Run `man fzf-tmux` to learn more about the available options.

### Custom Border Label

If you want to customize the fzf popup border label, you can add `T_FZF_BORDER_LABEL` to your shell variable

```bash
# ~/.bashrc or ~/.zshrc
export T_FZF_BORDER_LABEL=' Your Custom Label '
```

or if you use fish:

```fish
# ~/.config/fish/config.fish
set -Ux T_FZF_BORDER_LABEL " Your Custom Label "
```

## Background

Interested in learning more about how this script came to be? Check out [Smart tmux sessions with zoxide and fzf](https://www.joshmedeski.com/posts/smart-tmux-sessions-with-zoxide-and-fzf/).
]

## Bonus: macOS keyboard shortcut

My personal workflow uses [macOS Keyboard Shortcuts for tmux](https://www.joshmedeski.com/posts/macos-keyboard-shortcuts-for-tmux/). I have bound the `t` popup to `cmd+j` with the following code:

<details>
<summary>Alacritty</summary>

Add the following line to your `alacritty.yml`

```yml
key_bindings:
  - { key: K, mods: Command, chars: "\x02\x54" } # open t - tmux smart session manager
```

</details>

<details>
<summary>Kitty</summary>

Add the following line to your `kitty.conf`

```sh
map cmd+k send_text all \x02\x54
```

</details>

<details>
<summary>WezTerm</summary>

Add the following line to your `wezterm.lua` inside the **keys** options

```sh
{ key = 'j', mods = 'CMD', action = wezterm.action.SendString '\x02\x54' }, -- open t - tmux smart session manager

```

</details>

**Note:** These bindings are based off the default prefix, `ctrl+b` (which converts to `\x02`). If you changed your prefix, I recommend [watching my video](https://www.joshmedeski.com/posts/macos-keyboard-shortcuts-for-tmux/) which goes into depth how to customize your own keybindings in Alacritty.
