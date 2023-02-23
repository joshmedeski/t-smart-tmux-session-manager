<a href="https://www.joshmedeski.com/posts/smart-tmux-sessions-with-zoxide-and-fzf/" target="_blank">

![thumbnail](https://github.com/joshmedeski/t-smart-tmux-session-manager/blob/main/smart-tmux-sessions-with-zoxide-and-fzf.jpeg?raw=true)

</a>

# t - the smart tmux session manager

tmux is a powerful tool, but dealing with sessions can be painful. This script makes it easy to create tmux sessions.

## Prerequisites

- [tmux](https://github.com/tmux/tmux)
- [zoxide](https://github.com/ajeetdsouza/zoxide)
- [fzf](https://github.com/junegunn/fzf)

## How to install

### 1. Install tpm plugin

This script can be installed with the [Tmux Plugin Manager (tpm)](https://github.com/tmux-plugins/tpm).

Add the following line to your `~/.tmux.conf` file:

```conf
set -g @plugin 'joshmedeski/t-smart-tmux-session-manager'
```

### 2. Add to path

To use the `t` script from anywhere, select your shell environment and follow the instructions.

**Note:** you'll need to check the path of your tpm plugins. It may be `~/.tmux/plugins` or `~/.config/tmux/plugins` depending on where your `tmux.conf` is located.

<details>
<summary>bash</summary>

Add the following line to `~/.bashrc`

```fish
# ~/.tmux/plugins
export PATH=$HOME/.tmux/plugins/t-smart-tmux-session-manager/bin:$PATH
# ~/.config/tmux/plugins
export PATH=$HOME/.config/tmux/plugins/t-smart-tmux-session-manager/bin:$PATH
```

</details>

<details>
<summary>zsh</summary>

Add the following line to `~/.zprofile`

```fish
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

## How to use

By default, this plugin is bound to `<prefix>+T` which triggers a fzf-tmux popup that display zoxide results. Type the result you want and when you hit enter it will create a tmux session and connect to it or, if the sessions already exists, switch to it.

If you are not in tmux, you can simply run `t` to start the interactive script, or call `t {name}` to jump directly to a session of your choosing.

You can learn more about how the script works in [this video](https://www.youtube.com/watch?v=l_TTxc0AcCw).

## Tasks

- [x] Create tpm plugin
- [x] Merge scripts and reduce logic
- [x] Add docs
- [x] Add help flag with basic documentation (`t -h`)
- [x] List tmux sessions
- [ ] Publish YouTube video on how to install it
- [ ] Save zoxide entries selected from t script (with sqlite?)
- [ ] Allow user to overwrite options (ex: `set -g @t-smart-tmux-session-manager-options "-p --reverse`)
- [ ] Add Neovim Telescope support?
- [ ] Add fzf preview support?
- [ ] Create Raycast plugin
