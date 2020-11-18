# Setting up TMUX

## 1. Install TMUX

```text
sudo apt install tmux -y
```

### 1.1 Configuring TMUX

Session creation:

```text
tmux new -s session1
```

Where the session name `session1` can be written any.

Exit from the session using the keys:

> CTRL+B-&gt;D

#### Return to session:

```text
tmux attach -t session1
```

#### View working sessions:

```text
tmux ls
```

#### Deleting a session:

```text
tmux kill-session -t session1
```

