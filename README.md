# clearzilla

`clear` is boring. `paclear` gave us Pac-Man. clearzilla gives you Godzilla.

Type `clearzilla` and an ASCII Godzilla stomps into your terminal, fire-breathing away every line of on-screen content before leaving you with a clean slate. The rampage scales with how much is actually on screen — a few lines gets a quick blast; a full scrollback session gets the full kaiju treatment.

Zero dependencies. One Bash script. No build step.

```
clearzilla              # stomp in and clear
clearzilla -s 3         # 3× animation speed
clearzilla -f           # skip animation, clear immediately
```

## Install

```sh
curl -Lo ~/.local/bin/clearzilla https://raw.githubusercontent.com/Arrowstorm-Technologies-LLC/clearzilla/main/clearzilla
chmod +x ~/.local/bin/clearzilla
```

Make sure `~/.local/bin` is in your PATH:

```sh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc  # or ~/.zshrc
```

Or install via [rack](https://github.com/Arrowstorm-Technologies-LLC/rack) once a release is published:

```sh
rack clearzilla Arrowstorm-Technologies-LLC/clearzilla
```

## Usage

```
clearzilla              Stomp in and clear the screen
clearzilla -s <n>       Speed multiplier (default: 1, higher = faster)
clearzilla -f           Skip animation, clear immediately
clearzilla -h           Show help
clearzilla -v           Show version
```

### Alias it over `clear`

```sh
alias clear=clearzilla
```

Pipe and non-TTY contexts automatically fall back to an instant clear (no animation).

## How it works

1. Detects terminal size and how many lines of content are on screen (via cursor position).
2. Draws Godzilla's head — detailed profile, dorsal plates, open jaw — fixed on the **left** side, facing right.
3. A cone-shaped flame fans out from his mouth, sweeping **rightward** column by column.
4. Characters are cleared **only** when the fire front reaches them — untouched content stays visible until then.
5. The fan widens as it travels (turbulent edges, hot core, outer wisps, trailing embers).
6. Finishes with a full terminal reset.

More lines on screen → wider fan + slower sweep → longer rampage.

## Requirements

- Bash 4+
- A terminal that understands ANSI escape sequences (basically all of them)
- `stty` and `tput` (standard on any Linux/macOS system)

## License

MIT — see [LICENSE](LICENSE).