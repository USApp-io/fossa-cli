# Bazel

## Support

[Bazel](#https://www.bazel.build/) projects are supported in a limited fashion by leveraging known bazel build rules for go, rust, npm, python, and C.

- `bazel` is required to be installed in order to find the transitive dependency graph.
- A functioning Bazel project that has been built.
- (Optional) A `WORKSPACE` file is necessary for automatic discovery along with a `vendor` or `third_party` folder.

## Configuration

### Automatic

Running `fossa init` can detect Bazel projects 
1. if a `WORKSPACE` file is found:
2. Search for known vendor directories in the base directory. `vendor` and `third_party` are the current list..

### Manual

Add a module with `type: bazel`, `target` set to a valid bazel target) and `dir` set to the current directory.

```yaml
analyze:
  modules:
    - name:   WORKSPACE
      type:   bazel
      target: vendor/...
      dir:   project-dir
```

## Analysis

Bazel projects follow a very simple analysis process.
1. Run `bazel query 'deps(<target>)' --output xml`.
1. Parse the output for recognized rules for go, npm, python, rust, and C.
1. Parse each rule looking for custom information that is used to extract individual dependencies.
   1. In the case of C dependencies we will archive upload the whole dependency and directly analyze the code for license signatures.
1. Collect these dependencies and construct a dependency graph.