Dinky is a terminal-based text editor by Simon Edwards (github.com/sedwards2009/dinky), written in Go. 


I created a PKGBUILD that:
   - Builds from the upstream v0.8.0 source tarball (not our fork).
   - Uses `CGO_ENABLED=0` to match upstream's build configuration (pure Go, no C dependencies).
   - Applies `-buildmode=pie -trimpath -mod=readonly -modcacherw` for security hardening and reproducibility.
   - Includes a `check()` function (upstream has no tests currently, but the hook is in place).
   - Installs the binary to `/usr/bin/dinky` and the MIT license to `/usr/share/licenses/dinky/LICENSE`.
   - Maintainer listed as `robinpie`.

Generated `.SRCINFO` via `makepkg --printsrcinfo`.

Validated the package:
   - `makepkg -sf` — full build succeeded.
   - `namcap PKGBUILD` — clean, no warnings.
   - `namcap dinky-0.8.0-1-x86_64.pkg.tar.zst` — one warning about FULL RELRO, which is expected and unavoidable for pure Go binaries built with `CGO_ENABLED=0` (full RELRO requires external linkmode, which requires CGO).

Created a separate AUR-ready directory at `/run/media/robin/Robin_s Card/codingStuff/dinky-aur-pkg/` containing only `PKGBUILD` and `.SRCINFO`, since the AUR git repo should not contain upstream source code.

### Build decision: CGO_ENABLED=0

The Arch Go packaging guidelines recommend using CGO with external linkmode for full RELRO and debug package support. However, we chose `CGO_ENABLED=0` because:
- The upstream project explicitly builds this way (see `.goreleaser.yaml`).
- Dinky is pure Go with no C bindings.

Removed all upstream Dinky source code and build artifacts from this fork. This repo now serves purely as the downstream AUR packaging repo, containing only `PKGBUILD`, `.SRCINFO`, `README.md`, `LICENSE`, packaging documentation, and the images referred to by README.md.

Published `dinky` v0.8.0 to the AUR. Set up AUR account (robinpie), configured SSH key (`~/.ssh/aur`) with `~/.ssh/config` entry for `aur.archlinux.org`, and pushed to `ssh+git://aur@aur.archlinux.org/dinky.git`.
