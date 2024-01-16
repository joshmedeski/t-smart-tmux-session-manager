**Annoucement 🎉📣** I've rewriten this project in Go and called it "sesh", go [check it out!](https://github.com/joshmedeski/sesh)

# t - the smart tmux session manager

tmux is a powerful tool, but dealing with sessions can be painful. This script makes it easy to create and switch tmux sessions:

## Prerequisites

- [tmux](https://github.com/tmux/tmux) (>= 3.2)
- [tpm](https://github.com/tmux-plugins/tpm)
- [zoxide](https://github.com/ajeetdsouza/zoxide)
- [fzf](https://github.com/junegunn/fzf) (>=0.35.0)

_Note: some users have had issues with fzf integration on tmux 3.2a where upon
spawning fzf it would lock the tmux pane. Upgrading to 3.3a seems to be a viable
workaround_ Check [#104](https://github.com/joshmedeski/t-smart-tmux-session-manager/issues/104)

```sh
brew install tmux zoxide fzf
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

## Extra features

### Cloning repositories

You can quickly clone a repository to your preferred directory by using the `t` command combined with the `-r` flag (or `--repo`).

First, you have to set the `T_REPOS_DIR` variable in your shell environment. Make sure to set it where you want your repositories cloned.

<details>
<summary>bash</summary>

Add the following line to `~/.bashrc`

```sh
export T_REPOS_DIR="~/repos"
```

</details>

<details>
<summary>zsh</summary>

Add the following line to `~/.zshrc`

```sh
export T_REPOS_DIR="~/repos"
```

</details>

<details>
<summary>fish</summary>

Add the following line to `~/.config/fish/conf.d/t.fish`

```fish
set -Ux T_REPOS_DIR ~/repos
```

</details>

In order to use the feature, simply run:

```sh
t -r https://github.com/joshmedeski/t-smart-tmux-session-manager.git
```

**Note:** it has to be a valid git remote url (ending in `.git`) or order to work.

I prefer to copy the repository URL to my clipboard and run the following command on macOS.

<details>
<summary>bash/zsh</summary>

```sh
t -r $(pbpaste)
```

</details>

<details>
<summary>fish</summary>

```sh
t -r (pbpaste)
```

</details>

If you want to overwrite the directory to clone to, you can overwrite the `T_REPOS_DIR` variable before running the command:

```sh
T_REPOS_DIR=~/code t --repo https://github.com/joshmedeski/tmux-list.git
```

## How to customize

### Use Git Root for session name

You may prefer your session names starting from the root of the git repository. This can help with naming conflicts if you have multiple directories with the same name on your machine and make it clear when you have multiple sessions open in the same git repository.

<details>
<summary>bash</summary>

Add the following line to `~/.bashrc`

```sh
export T_SESSION_USE_GIT_ROOT="true"
```

</details>

<details>
<summary>zsh</summary>

Add the following line to `~/.zshrc`

```sh
export T_SESSION_USE_GIT_ROOT="true"
```

</details>

<details>
<summary>fish</summary>

Add the following line to `~/.config/fish/conf.d/t.fish`

```fish
set -Ux T_SESSION_USE_GIT_ROOT true
```

</details>

### Include parent dir in session name

You may prefer your session names having a prefix of the parent directory. This can help with naming conflicts if you have multiple directories with the same name on your machine.

<details>
<summary>bash</summary>

Add the following line to `~/.bashrc`

```sh
export T_SESSION_NAME_INCLUDE_PARENT="true"
```

</details>

<details>
<summary>zsh</summary>

Add the following line to `~/.zshrc`

```sh
export T_SESSION_NAME_INCLUDE_PARENT="true"
```

</details>

<details>
<summary>fish</summary>

Add the following line to `~/.config/fish/conf.d/t.fish`

```fish
set -Ux T_SESSION_NAME_INCLUDE_PARENT true
```

</details>

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

## Startup script

You can run a startup script when you create a new session. This is useful for running a command when you create a new session, like starting a dev server or automatically opening neovim to begin editing a file.

This works by adding a `.t` file to your desired directory. Here is a quick script for bootstrapping that file:

```sh
touch .t && chmod +x .t && echo -e '#!/usr/bin/env bash\n' > .t && nvim .t
```

I like opening Neovim and the find file Telescope prompt to quickly find a file to edit. Here is an example of what I put in many of my projects:

```sh
#!/usr/bin/env bash
nvim -c 'Telescope find_files'
```

So, when you open any project that detects a `.t` it will automatically run that script when a session is created.

This feature is in early development so please feel free to give feedback if you have ideas for how to improve on it.

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
