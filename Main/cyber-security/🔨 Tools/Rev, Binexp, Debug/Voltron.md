---
tags: tool
---
# Voltron ⚡️

Debugger UI toolkit written in #python, provides a GUI for various debuggers, for example [[GDB(gef)]], [[LLDB]], [[VDB]], [[WinDBG]].

Built in view for:
- Registers
- Disassembly
- Stack
- Memory
- Breakpoints
- Backtrace

## Supported platforms
||lldb|gdb|vdb|windbg|
|---|---|---|---|---|
|x86|✓|✓|✓|✓|
|x86_64|✓|✓|✓|✓|
|arm|✓|✓|✓|✗|
|arm64|✓|✗|✗|✗|
|powerpc|✗|✓|✗|✗|

## Installation

__Blackarch:__
```bash
sudo pacman -S voltron
```

__Manual:__
```bash
git clone https://github.com/snare/voltron
cd voltron
./install.sh
```


More at: https://github.com/snare/voltron
