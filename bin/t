#!/bin/bash

if [ "$1" = "-h" ]; then
  echo ""
  echo "\033[1m  t - the smart tmux session manager\033[0m"
  echo "\033[37m  https://github.com/joshmedeski/t-smart-tmux-session-manager"
  echo ""
  echo "\033[32m  Run interactive mode"
  echo "\033[34m      t"
  echo ""
  echo "\033[32m  Go to session"
  echo "\033[34m      t {name}"
  echo ""
elif [ $# -eq 0 ]; then
  if [ -z "$TMUX" ]; then
    ZOXIDE_RESULT=$(zoxide query -l | fzf --reverse --header 'tmux-session')
  else
    ZOXIDE_RESULT=$(zoxide query -l | fzf-tmux -p --reverse --header 'tmux-session')
  fi
else
  ZOXIDE_RESULT=$(zoxide query $1)
fi

if [ -z "$ZOXIDE_RESULT" ]; then
  exit 0
fi

FOLDER=$(basename $ZOXIDE_RESULT)
SESSION_NAME=$(echo $FOLDER | tr ' ' '_' | tr '.' '_')

# lookup tmux session name
SESSION=$(tmux list-sessions | grep $SESSION_NAME | awk '{print $1}')
SESSION=${SESSION//:/}

# if not currently in tmux
if [ -z "$TMUX" ]; then
  # tmux is not active
  if [ -z "$SESSION" ]; then
    # session does not exist
    # jump to directory
    cd $ZOXIDE_RESULT
    # create session
    tmux new-session -s $SESSION_NAME
  else
    # session exists
    # attach to session
    tmux attach -t $SESSION
  fi
else
  # tmux is active
  if [ -z "$SESSION" ]; then
    # session does not exist
    # jump to directory
    cd $ZOXIDE_RESULT
    # create session
    tmux new-session -d -s $SESSION_NAME
    # attach to session
    tmux switch-client -t $SESSION_NAME
  else
    # session exists
    # attach to session
    # switch to tmux session
    tmux switch-client -t $SESSION
  fi
fi
