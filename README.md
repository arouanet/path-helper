# path-helper

[![MELPA](https://melpa.org/packages/path-helper-badge.svg)](https://melpa.org/#/path-helper)
[![MELPA Stable](https://stable.melpa.org/packages/path-helper-badge.svg)](https://stable.melpa.org/#/path-helper)

This package replicates the behavior of the
[path_helper](https://opensource.apple.com/source/shell_cmds/shell_cmds-203/path_helper/)
utility shipped with macOS, used to augment the `PATH` and `MANPATH` environment
variables with path elements read respectively from `/etc/paths` and
`/etc/paths.d/` for `PATH`, and from `/etc/manpaths` and `/etc/manpaths.d/` for
`MANPATH`.

This is needed because macOS GUI applications are launched with a limited
default environment which is not set from the user's login shell. While it is
possible to configure `launchd` to use a different `PATH` variable (using
`launchctl config user path`), another solution is to set the PATH after the
fact during Emacs initialization, which is what this package is doing.

This is similar to what the
[exec-path-from-shell](https://github.com/purcell/exec-path-from-shell) package
does, but because path-helper does not need to spawn a shell to read the
environment, it is much faster.

An obvious downside of this approach is that if the PATH is manually set
elsewhere, such as in the user `.profile` file, it will not be visible to
path-helper. But properly configured macOS packages such as
[MacTeX](http://www.tug.org/mactex/), which contribute to the PATH by adding
a file in `/etc/paths.d/`, will work as expected.

## Usage

To set all environment variables listed in `path-helper-variables`, add the
following to your init file:

    (when (memq window-system '(ns mac))
      (path-helper-setenv-all))

Or, if using [use-package](https://github.com/jwiegley/use-package):

    (use-package path-helper
      :if (memq window-system '(mac ns))
      :ensure t
      :config
      (path-helper-setenv-all))

The list `path-helper-variables` is set to `("PATH" "MANPATH")` by default, but
you can customize it to just `("PATH")` for example.

You can also call `path-helper-setenv` directly to set a single environment
variable, e.g.:

    (path-helper-setenv "PATH")
