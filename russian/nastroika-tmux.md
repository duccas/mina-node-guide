# Настройка TMUX

## 1. Установка TMUX

```text
sudo apt install tmux -y
```

### 1.1 Настройка TMUX

Создание сессии:

```text
tmux new -s session1
```

Где имя сессии `session1` можно написать любое.

Выход из сессии осуществляется клавишами:

> CTRL+B-&gt;D

#### Вернуться в сессию:

```text
tmux attach -t session1
```

#### Посмотреть рабочие сессии:

```text
tmux ls
```

#### Удаление сессии:

```text
tmux kill-session -t session1
```

