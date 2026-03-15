# Consoles
Linux / Mac - iTerm
Windows - git bash / MSYS2

tmux:
	create new window: ctrl b + c
	switch windows: ctrl b + number
	exit the window: ctrl d

`cygpath`: a command-line utility for Windows that converts file paths between Windows-style (e.g., `C:\path`) and POSIX/Unix-style (e.g., `/cygdrive/c/path`). It is essential for scripts passing filenames between native Windows applications and Unix-like environments. 
**Key `cygpath` Options:**
- **`-u`, `--unix`**: Convert Windows or mixed paths to Unix/POSIX format.
- **`-w`, `--windows`**: Convert Unix paths to Windows format (backslash `\`).
- **`-m`, `--mixed`**: Convert Unix paths to Windows format using forward slashes (`/`), often preferred for compatibility.
- **`-p`, 
    `--path`
    **
    : Convert path-style strings (e.g., separating by colon vs. semicolon)
    .
- **`-a`, `--absolute`**: Output absolute paths.
# Contents
[[Bash Basics]]
[[Users & Groups]]
[[Data Wrangling]]
	[[Vim]]
[[Disk Operations]]
[[Linux Network]]
[[Linux Process]]