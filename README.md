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
<summary>bash</summary>

Add the following line to `~/.bashrc`

```sh
# ~/.tmux/plugins
export PATH=$HOME/.tmux/plugins/t-smart-tmux-session-manager/bin:$PATH
# ~/.config/tmux/plugins
export PATH=$HOME/.config/tmux/plugins/t-smart-tmux-session-manager/bin:$PATH
```

</details>

<details>
<summary>zsh</summary>

Add the following line to `~/.zprofile`

```sh
# ~/.tmux/plugins
export PATH=$HOME/.tmux/plugins/t-smart-tmux-session-manager/bin:$PATH
# ~/.config/tmux/plugins
export PATH=$HOME/.config/tmux/plugins/t-smart-tmux-session-manager/bin:$PATH
```

</details>

<details>
<summary>fish</summary>

Add the following line to `~/.config/fish/config.fish`

```fish
# ~/.tmux/plugins
fish_add_path $HOME/.tmux/plugins/t-smart-tmux-session-manager/bin
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

```sh
  Run interactive mode
      t
        ctrl-s list only tmux sessions
        ctrl-x list only zoxide results
        ctrl-f list results from the find command

  Go to session (matches tmux session, zoxide result, or directory)
      t {name}

  Open popup (while in tmux)
      <prefix>+T
        ctrl-s list only tmux sessions
        ctrl-x list only zoxide results
        ctrl-f list results from the find command

  Show help
      t -h
      t --help
```

By default, this plugin is bound to `<prefix>+T` which triggers a fzf-tmux popup that display zoxide results. Type the result you want and when you hit enter it will create a tmux session and connect to it or, if the sessions already exists, switch to it.

If you are not in tmux, you can simply run `t` to start the interactive script, or call `t {name}` to jump directly to a session of your choosing.

### Key Bindings

- `ctrl-s` list only tmux sessions
- `ctrl-x` list only zoxide results
- `ctrl-f` find by directory

## How to customize

### Custom fzf-tmux keybinding

By default, the `t` popup is bound to `<prefix>T`. In order to overwrite your own custom key binding, add setting the `@t-bind` varaible to your `tmux.conf`:

```sh
set -g @t-bind "K"
```

You can unbind the default by using `none`.

```sh
set -g @t-bind "none" # unbind default
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

**Note:** These bindings are based off the default prefix, `ctrl+b` (which converts to `\x02`). If you changed your prefix, I recommend [watching my video](https://www.joshmedeski.com/posts/macos-keyboard-shortcuts-for-tmux/) which goes into depth how to customize your own keybindings in Alacritty.
