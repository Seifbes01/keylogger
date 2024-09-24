# DLL Injection Keylogger

This project implements a basic keylogger using DLL injection in Python. It utilizes `ctypes` to interface with Windows API functions to hook into keyboard events, log keypresses, and capture active window titles. This could be used for educational purposes to understand how Windows API works and how to hook keyboard events.

**⚠️ Disclaimer: This code is for educational purposes only. Unauthorized use of keyloggers is illegal and unethical. Do not use this project for malicious purposes.**

## Features

- Captures keypresses using a low-level keyboard hook (`WH_KEYBOARD_LL`).
- Logs each keypress and prints it to the console.
- Detects when the foreground window changes and logs the window title.
- Supports logging shifted characters.
- Implements `ToAscii` to convert virtual key codes to ASCII characters.

## Prerequisites

- Python 3.x
- Windows OS
- Admin rights may be required for hooking into system functions.

## Dependencies

The project uses `ctypes` to interface with Windows API. No external libraries are needed besides standard Python.

## How It Works

1. **Keyboard Hook**: The project uses `SetWindowsHookExA` to install a low-level keyboard hook. This allows capturing all keyboard events on the system.
2. **Foreground Window Detection**: The function `get_foreground_process()` is used to detect the currently active window. Each time the window changes, its title is printed.
3. **Key Press Logging**: Keys are detected and converted from virtual key codes to ASCII characters using `ToAscii`. Each key is printed to the console, and special characters (like `ENTER`) are handled appropriately.

## Code Explanation

### Key Components

- **Keyboard Hook**: 
    ```python
    hook = SetWindowsHookExA(WH_KEYBOARD_LL, callback, 0, 0)
    ```
    Hooks into the system to capture keyboard events.

- **Foreground Window Title**: 
    ```python
    def get_foreground_process():
        hwnd = user32.GetForegroundWindow()
        length = GetWindoWTextLengthA(hwnd)
        buff = create_string_buffer(length + 1)
        GetWindowTextA(hwnd, buff, length + 1)
        return buff.value
    ```
    Retrieves and returns the title of the active window.

- **Key Press Capture**:
    ```python
    def hook_function(nCode, wParam, lParam):
        if wParam == WM_KEYDOWN:
            # Process key press
    ```

    This function processes each key press and converts the virtual key code to its corresponding ASCII character.

## How to Run

1. Clone this repository:
    ```bash
    git clone https://github.com/Seifbes01/keylogger.git
    ```

2. Run the script:
    ```bash
    python keylogger.py
    ```

3. The keylogger will start and print keypresses in real-time, along with the current foreground window.

## Stopping the Keylogger

You cannot stop the keylogger using `Ctrl + C`. Instead, use one of the following methods:

1. **`Ctrl + Pause/Break`**: This will stop the execution of the keylogger.
2. **Close the CLI Window**: You can also close the command-line interface (CLI) window to stop the keylogger.

To implement a more graceful shutdown with `KeyboardInterrupt`, you can modify the script to handle `Ctrl + C` and exit cleanly.

### Example of Handling `Ctrl + C`:

You can add a try-except block around the main loop to capture `KeyboardInterrupt`

## Notes

- Make sure you have admin privileges to run this script if needed.
- The script captures and logs keypresses to the console. You can modify it to save the logs to a file if needed.
  
