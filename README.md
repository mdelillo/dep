# Dep Cloud Native Buildpack

The Go Dep CNB builds a Go application binary, using the
[`dep`](https://golang.github.io/dep/docs/introduction.html) tool to package
dependencies.

## Integration

The Dep CNB provides dep as a dependency. Downstream
buildpacks can require the node dependency by generating a [Build Plan
TOML](https://github.com/buildpacks/spec/blob/master/buildpack.md#build-plan-toml)
file that looks like the following:

```toml
[[requires]]

  # The name of the Dep dependency is "dep". This value is
  # considered part of the public API for the buildpack and will not change
  # without a plan for deprecation.
  name = "dep"

  # Note: The version field is unsupported as there is no version for a set of
  # dep.

  # The Dep buildpack supports some non-required metadata options.
  [requires.metadata]

    # Setting the build flag to true will ensure that the Dep
    # depdendency is available on the $PATH for subsequent buildpacks during
    # their build phase. If you are writing a buildpack that needs to run Dep
    # during its build process, this flag should be set to true.
    build = true
```

## Usage

To package this buildpack for consumption:
```
$ ./scripts/package.sh
```
This builds the buildpack's Go source using GOOS=linux by default. You can supply another value as the first argument to package.sh.

## `buildpack.yml` Configuration

The `dep` requires a `buildpack.yml` file in the root of the application directory, and must contain the `import-path` directive.

```yaml
go:
  # this sets the go import-path (required)
  import-path: hubgit.net/user/app

  # this allows you to override the location of the main package of the app
  targets: ["./cmd/web"]

  # this allows you to set Go ldflags for compilation
  ldflags:
    main.version: v1.2.3
    main.sha: 1234567
```

See `integration/testdata/` subfolders for examples.
