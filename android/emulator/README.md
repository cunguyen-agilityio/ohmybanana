# Resolving "Failed to Launch Emulator" Issue on macOS

If you encounter the following error when trying to start an Android emulator on macOS:

```bash
error Failed to launch emulator. Reason: No emulators found as an output of `emulator -list-avds`
```

This issue occurs because the system cannot locate your Android emulator's path. Follow these steps to fix it:

## Solution

**Step 1: Update Your PATH Environment Variable**

1. Open your terminal.

2. Run the following command to add the Android emulator directory to your PATH environment variable:

```bash
echo 'export PATH=$PATH:~/Library/Android/sdk/emulator/' >> ~/.bash_profile
```

This command appends the emulator's directory to the PATH in your `~/.bash_profile` file.

**Step 2: Apply the Changes**

Reload your `~/.bash_profile` file to apply the changes by running:

```bash
source ~/.bash_profile
```

**Step 3: Verify the Emulator Availability**

Check if your system recognizes the available emulators by running:

```bash
emulator -list-avds
```

This command should return a list of your available emulators. If the list appears, the issue is resolved.

## Additional Notes

- Ensure that you have at least one emulator created in Android Studio. You can create a new emulator by navigating to **Tools > Device Manager** in Android Studio and following the prompts.

- If you're using a shell other than bash (e.g., `zsh`), replace `~/.bash_profile` with the corresponding profile file (e.g., `~/.zshrc`).

By following these steps, you should be able to launch your Android emulator successfully.
