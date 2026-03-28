# Portable Ruby Binaries

Tools to build versions of Ruby that can be installed and run from anywhere on the filesystem.

This is a fork of [jdx/ruby](https://github.com/jdx/ruby) with **immutable releases**: each Ruby version is published once at a stable, versioned tag (e.g. `4.0.1-1`) and never modified. This ensures downstream tools like [bazel-contrib/rules_ruby](https://github.com/bazel-contrib/rules_ruby) can pin checksums without breakage.

## Releases

Releases are tagged as `X.Y.Z-1` (e.g. `4.0.1-1`). The tag is immutable — the binaries at that tag never change. If a rebuild is needed, a new tag is created (`4.0.1-2`, etc.).

Artifacts inside each release are named by Ruby version and platform:

| Platform | Artifact |
|---|---|
| macOS ARM64 | `ruby-X.Y.Z.arm64_darwin.tar.gz` |
| macOS x86_64 | `ruby-X.Y.Z.x86_64_darwin.tar.gz` |
| Linux x86_64 | `ruby-X.Y.Z.x86_64_linux.tar.gz` |
| Linux ARM64 | `ruby-X.Y.Z.aarch64_linux.tar.gz` |

Download from the [releases page](https://github.com/bazel-contrib/portable-ruby/releases) and extract to any location.

## Release automation

New Ruby versions are picked up automatically:

1. **autobump** runs every 12 hours, creates a formula for any new Ruby release from [ruby/ruby](https://github.com/ruby/ruby)
2. **release-new** runs every 12 hours, finds formulae without a release and triggers the build pipeline
3. **release** builds on macOS (ARM64 + x86_64) and Linux (x86_64 + ARM64), then publishes an immutable release

To manually trigger a release:

```sh
gh workflow run release-new.yml \
  --repo bazel-contrib/portable-ruby \
  -f version=4.0.1
```

To force a rebuild of an existing version:

```sh
gh workflow run release-new.yml \
  --repo bazel-contrib/portable-ruby \
  -f version=4.0.1 \
  -f replace_all=true
```

## Local development

- Run `bin/setup` to tap your checkout of this repo as `jdx/ruby`.
- Run e.g. `brew jdx-package --no-uninstall-deps --debug --verbose jdx-ruby@3.4.5` to build Ruby 3.4.5 locally with YJIT.

## Thanks

Forked from [jdx/ruby](https://github.com/jdx/ruby), which was forked from [spinel-coop/rv-ruby](https://github.com/spinel-coop/rv-ruby), originally based on [Homebrew/homebrew-portable-ruby](https://github.com/Homebrew/homebrew-portable-ruby).

## License

Code is under the [BSD 2-Clause "Simplified" License](/LICENSE.txt).
