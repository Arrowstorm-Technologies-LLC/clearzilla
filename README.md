# clearzilla

`clear` is boring. `paclear` gave us Pac-Man. clearzilla gives you Godzilla.

Type `clearzilla` and a Unicode block-art Godzilla stomps across your terminal, sweeping the screen band by band before a final reset leaves you with a clean slate. Taller terminals get more passes — Godzilla keeps marching until every row has been stomped.

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

Or install via [rack](https://github.com/Arrowstorm-Technologies-LLC/rack):

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

### Run something after the clear

If your shell reprints a banner, MOTD, or prompt after plain `clear` (a shell
function override, usually), clearzilla can drive the same thing once it
finishes stomping:

```sh
my_post_clear() { echo "welcome back"; }
export -f my_post_clear
export CLEARZILLA_POST_CLEAR=my_post_clear
```

`CLEARZILLA_POST_CLEAR` names an exported shell function or any command on
`PATH`; clearzilla runs it right after the screen clears, in both the
animated and `-f`/non-TTY paths. Unset or pointing at nothing found, it's a
no-op. A failing hook never aborts clearzilla.

## How it works

Inspired by [paclear](https://github.com/orangekame3/paclear), clearzilla uses a tall multi-line Godzilla sprite (24 rows) that sweeps the terminal in horizontal bands:

1. Reads terminal dimensions via `stty` / `tput`.
2. Steps down the screen in bands, continuing until every row has been covered (including any partial band at the bottom).
3. Each band: Godzilla walks left-to-right (bands alternate direction), erasing the sprite footprint as it moves. The sweep deliberately stops one column short of the terminal's last column, since a band covering the last row would otherwise write to the terminal's absolute bottom-right cell -- a classic ANSI edge case that can make a terminal register a deferred wrap and scroll unexpectedly on the next write.
4. Finishes with a full terminal reset (`clear`).

More terminal rows → more bands to sweep → longer rampage.

Fire-breath and ash-debris effects are implemented but currently disabled in the shipped script — the walk-and-stomp sweep plus the final reset is what runs today.

## Requirements

- Bash 4+
- A UTF-8 locale (for the block-art sprite)
- A terminal that understands ANSI escape sequences
- `stty` and `tput` (standard on Linux and macOS)

## License

MIT — see [LICENSE](LICENSE).