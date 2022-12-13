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

This script can be installed with the [Tmux Plugin Manager (tpm)](https://github.com/tmux-plugins/tpm).

Add the following line to your `~/.tmux.conf` file:

```conf
set -g @plugin 'joshmedeski/t-smart-tmux-session-manager'
```

## How to use

```
  Go to session
      t {name}

  Run interactive mode
      t
```
