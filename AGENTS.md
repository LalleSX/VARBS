# AGENTS.md — VARBS

## Project Overview

**VARBS (Victor's Auto-Rice Bootstrapping Scripts)** is a personal, automated Arch Linux setup inspired by Luke Smith's LARBS.

The goal is to create a minimal, fast, reliable, reproducible, and highly customizable Linux desktop environment built around Wayland and suckless-inspired software.

VARBS should transform a minimal Arch Linux installation into a fully functional, personalized desktop with as little manual intervention as reasonably possible.

The project is primarily developed and maintained with the assistance of AI coding agents such as OpenAI Codex.

## Core Principles

1. **Simplicity:** Prefer simple, understandable solutions over unnecessary abstraction.
2. **Minimalism:** Avoid unnecessary dependencies, background services, and software.
3. **Automation:** Automate repetitive installation, configuration, and maintenance tasks.
4. **Reproducibility:** A fresh Arch Linux installation should produce a consistent desktop environment.
5. **Reliability:** Stability and maintainability take priority over visual customization.
6. **Performance:** Favor lightweight software and avoid unnecessary resource consumption.
7. **Customization:** Keep the system easy to modify without sacrificing maintainability.
8. **User Control:** Never make irreversible system changes without explicit authorization.

Follow the suckless philosophy where practical, but do not sacrifice usability merely to minimize lines of code or dependencies.

## Target Environment

- **Distribution:** Arch Linux (rolling release)
- **Architecture:** x86_64
- **Display protocol:** Wayland
- **Compositor:** dwl
- **Terminal:** foot
- **Application launcher:** fuzzel
- **Status bar:** somebar (initial preference)
- **Notifications:** mako
- **Audio:** PipeWire and WirePlumber
- **Network management:** NetworkManager
- **Shell:** Zsh
- **Editor:** Neovim
- **File manager:** lf
- **Dotfile management:** GNU Stow
- **Package management:** pacman and an AUR helper
- **Primary GPU:** NVIDIA GPU
- **CPU:** AMD

These are initial architectural choices, not immutable requirements. Changes must be justified and documented.

## Desktop Experience

The desktop should be keyboard-centric without being keyboard-exclusive.

Requirements:

- Full keyboard-driven window management.
- Mouse-based window focus and interaction.
- Mouse-driven window movement and resizing.
- Clickable workspace tags where supported.
- A practical status bar with system information.
- Application launching through keyboard shortcuts.
- Functional notifications, audio controls, and clipboard.
- Multi-monitor support.
- High-refresh-rate display support.
- XWayland compatibility for legacy applications.

Avoid excessive animations, unnecessary visual effects, and complex desktop frameworks.

A clean, responsive, functional desktop is more important than visual novelty.

## NVIDIA and Wayland

NVIDIA compatibility is a critical project requirement.

Agents must:

- Prefer current Arch Linux recommendations for NVIDIA drivers.
- Account for DRM/KMS and explicit synchronization support.
- Verify compatibility between dwl, wlroots, and the installed NVIDIA driver.
- Avoid outdated environment-variable workarounds.
- Preserve the ability to recover from a broken graphical session.
- Document hardware-specific configurations.
- Never assume successful compilation guarantees a working graphical session.

Do not claim that hardware compatibility has been verified without testing it on the target machine.

## Repository Architecture

Use a modular structure similar to:

```text
VARBS/
├── AGENTS.md
├── README.md
├── install.sh
├── update.sh
├── packages/
├── scripts/
├── suckless/
├── dotfiles/
└── tests/
```

Keep installation logic, package lists, configuration files, and application source code separated.

Do not introduce unnecessary directory structures or frameworks.

## Bootstrap Requirements

The bootstrap process must:

1. Verify that the system is a supported Arch Linux environment.
2. Check prerequisites and required permissions.
3. Install required packages using pacman.
4. Install AUR packages when necessary.
5. Retrieve and build custom software.
6. Deploy configuration files.
7. Configure required user services.
8. Validate installation results.
9. Report failures and required manual actions.

The installation must be **idempotent** wherever practical.

Running the installer multiple times should not duplicate configuration, corrupt existing files, or unnecessarily rebuild unchanged components.

Use clear error messages and fail safely.

Do not perform disk partitioning, formatting, bootloader installation, or destructive storage operations as part of the standard desktop bootstrap.

## Shell Scripting Standards

- Use Bash for installation and maintenance scripts.
- Start Bash scripts with `#!/usr/bin/env bash`.
- Prefer `set -euo pipefail` where appropriate.
- Quote variables correctly.
- Use functions for reusable operations.
- Check dependencies before invoking them.
- Avoid unnecessary subprocesses.
- Use explicit paths when appropriate.
- Make scripts safe to rerun.
- Use `sudo` only for operations requiring elevated privileges.

Never execute unverified remote scripts directly through a shell pipeline.

Prefer official repositories and trusted upstream sources.

## Package Management

- Prefer official Arch Linux packages.
- Use AUR packages only when necessary.
- Maintain explicit package lists.
- Avoid unnecessary dependencies.
- Do not install multiple applications serving the same purpose without justification.
- Never use `pacman -Sy` as a standalone installation or update operation.
- Never use `pacman -Rdd` to bypass dependency checks.
- Do not automatically remove unrelated or user-installed packages.

Document dependencies required to build custom software.

## dwl Development

dwl is the primary Wayland compositor and a central part of VARBS.

Agents should:

- Maintain dwl configuration and customizations in version control.
- Prefer minimal, understandable changes.
- Keep compatibility with the selected upstream version.
- Verify patches against the actual source revision.
- Compile and test changes before installation.
- Preserve the last known working build.
- Document keyboard shortcuts and mouse bindings.
- Avoid accumulating unnecessary patches.

Do not automatically track untested upstream development branches.

Use pinned source revisions or tested releases where practical.

## Dotfile Management

Use GNU Stow or an equally simple, explicitly approved alternative.

Requirements:

- Keep configuration files under version control.
- Preserve the expected directory hierarchy.
- Avoid overwriting existing user configuration without permission.
- Detect and report symlink conflicts.
- Never commit credentials, tokens, SSH private keys, or other secrets.
- Prefer portable configuration when practical.
- Keep hardware-specific configuration separate from general configuration.

## AI Agent Workflow

AI agents are expected to actively develop, test, refactor, and maintain this repository.

Before making changes:

1. Inspect relevant files.
2. Understand existing conventions.
3. Identify affected components.
4. Prefer the smallest maintainable solution.

When implementing changes:

1. Modify the appropriate source files.
2. Keep changes focused.
3. Update relevant documentation.
4. Run available tests and build checks.
5. Report the results.

After making changes, summarize:

- What changed.
- Why it changed.
- What was tested.
- Whether the tests passed.
- Any remaining risks or manual steps.

Do not claim that tests passed unless they were actually executed.

## Testing

Testing is mandatory where practical.

At minimum:

- Validate Bash syntax using `bash -n`.
- Run ShellCheck when available.
- Verify custom software builds.
- Check configuration syntax where supported.
- Test installation scripts in a safe environment.
- Check that scripts handle repeated execution.

WSL may be used for development and compilation, but it must not be treated as proof that the complete graphical environment works on native Arch Linux.

Use a virtual machine or the target hardware for graphical integration testing.

Never run destructive tests against the user's actual disks.

## Safety and Permissions

Agents may autonomously:

- Read and analyze repository files.
- Create and edit project files.
- Refactor scripts and configuration.
- Compile software.
- Run non-destructive tests.
- Update project documentation.

Agents must request explicit approval before:

- Deleting user data.
- Formatting or repartitioning disks.
- Modifying bootloaders or EFI entries.
- Changing existing storage mounts.
- Replacing working system configurations outside the repository.
- Removing system packages or services.
- Performing other potentially destructive system operations.

Treat the host system as valuable and potentially irreplaceable.

## Git Practices

- Keep commits focused and descriptive.
- Avoid committing generated binaries.
- Never commit secrets.
- Do not force-push or rewrite shared history without permission.
- Preserve user changes.
- Use branches for experimental or high-risk changes.
- Document significant architectural decisions.

## Documentation

The README should explain:

- What VARBS is.
- Supported environments.
- Installation instructions.
- Required prerequisites.
- How to update the system.
- How to customize dwl.
- How to recover from a failed installation.
- Known limitations.

Documentation should remain concise, accurate, and practical.

## Decision Priorities

When evaluating competing solutions, use this priority order:

1. System safety and data preservation.
2. Reliability and compatibility.
3. Simplicity and maintainability.
4. Reproducibility and automation.
5. Performance and resource efficiency.
6. User experience.
7. Visual appearance.

These priorities may be overridden by explicit user requirements, except where doing so would violate safety constraints.

## Final Objective

VARBS should make it possible to install a minimal Arch Linux system, clone this repository, execute a bootstrap script, and obtain a complete, usable, personalized Wayland desktop.

The user should spend time **using and improving Linux**, not repeatedly repairing or manually reconstructing the environment.

**Build a system that is easy to install, easy to understand, easy to modify, and difficult to break.**
