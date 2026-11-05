# Task Focuser Rust

Task Focuser is a small floating note for the one to three lines you are focusing on. Lennie the budgie and Meggie the owl keep it company. The Rust app runs on Windows, macOS, and Linux.

This is the Rust rewrite by [Vit Chrubasik](https://github.com/vichr-vita), from [pull request #1](https://github.com/Omni-Wild/task-focuser/pull/1) on the original [Task Focuser](https://github.com/Omni-Wild/task-focuser), a Windows app. It has its own repository so the original stays as it is.

<p align="center"><a href="docs/task-focuser-demo.mp4"><img src="docs/demo.gif" width="360" alt="The original Task Focuser note and bird. Select to open the video."></a></p>
<p align="center"><a href="docs/task-focuser-demo.mp4">Open the demo video, with sound</a></p>

See [migration notes](docs/RUST-MIGRATION.md) for state compatibility, paths, and platform limitations. The demo shows the original Windows app.

## Download

CI builds the app on every push. Sign in to GitHub, open the [latest CI run on `main`](https://github.com/Omni-Wild/task-focuser-rust/actions/workflows/ci.yml?query=branch%3Amain), and download the artifact for your system from the bottom of the page: `task-focuser-macOS-ARM64` (Apple silicon), `task-focuser-Windows-X64`, or `task-focuser-Linux-X64`. There is no Intel Mac build. Artifacts expire after 90 days.

On a Mac, unpack the download in Terminal, clear the download flag because the app is not notarized, and start it:

```sh
tar -xzf task-focuser-macOS-ARM64.tar.gz
xattr -d com.apple.quarantine task-focuser
./task-focuser
```

If your browser did not unzip the artifact, run `unzip task-focuser-macOS-ARM64.zip` first. If `xattr` reports no such attribute, the flag is already gone and you can continue.

## Use the note

Double-click the locked note to edit it. Press `Esc` or `Ctrl+L`, or click the padlock, to lock it again. While editing, use `Ctrl+=` and `Ctrl+-` to change the font size. On macOS, use the Command key in place of Ctrl.

Drag the note to move it. Drop it on the dot at the top of the screen to tuck it away, then double-click the dot to restore it. Drag the dot to the close target to hide both. The tray menu can show the note again or quit the app.

The editing bar controls the note's appearance, bird, and sound. Click the bird's name to choose Lennie or Meggie or rename them. Click the sound control to toggle sound, or hover over it to adjust volume and idle sounds. The bird's quiet idle sounds can keep Bluetooth headphones awake during silent stretches.

## Build and run

Install Rust with [rustup](https://rustup.rs/). The repository selects Rust 1.94.0 through `rust-toolchain.toml`. On Ubuntu, install the native development libraries first:

```sh
sudo apt-get install pkg-config libasound2-dev libudev-dev libwayland-dev libxkbcommon-dev libfontconfig1-dev libx11-dev libxrandr-dev libxi-dev libgl1-mesa-dev
```

Build and run the Rust app with:

```sh
cargo run --locked -p task-focuser
```

For a release build, run `scripts/build.sh` on Linux or macOS, or `scripts/build.ps1` in PowerShell on Windows. The binary is in `target/release/`. Run `scripts/check.sh` to check formatting, lints, and tests. CI checks the workspace on all three platforms and uploads release binaries as workflow artifacts. Its Linux job also exercises the native Wayland window under headless Sway. Windows and macOS jobs compile and run automated tests; they do not exercise the desktop UI.

Use `--help` to list the note commands and options. `--backend auto|native|wayland` selects the desktop host. On Linux, auto selects Wayland when available; that host requires layer-shell support. Use `--backend native` in an X11 session. `--no-tray` and `--no-sound` are available only for isolated runs.

For testing without changing the default note, set `TASKFOCUSER_DATA_DIR` to a disposable directory or set `TASKFOCUSER_PROFILE` to a separate profile name. Isolated runs cannot change login startup settings. See the migration notes for the default state paths.

The original WPF source is available in [Git history](https://github.com/Omni-Wild/task-focuser/tree/67b0db1/app).

## License

MIT. See [LICENSE](LICENSE).
