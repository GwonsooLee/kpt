---
title: "Exec Runtime"
linkTitle: "Exec Runtime"
weight: 2
type: docs
description: >
   Writing and running functions as executables
---

Functions may alternatively be run as executables outside of containers.  Exec
functions read input and write output the same as container functions, but are
run outside of a container.

Running functions as executables can be useful for function development, or for
running trusted executables.

Exec functions are disabled by default, and must be enabled with `--enable-exec`.

{{% pageinfo color="info" %}}
Exec functions may be converted to container functions by building the executable
into a container and invoking it as the container `CMD`.
{{% /pageinfo %}}

## Imperative Run

Exec functions may be run imperatively using the `--exec-path` flag with `kpt fn run`.

```sh
kpt fn run DIR/ --enable-exec --exec-path /path/to/executable
```

This is similar to building `/path/to/executable` into the container image
`gcr.io/project/image:tag` and running -- except that the executable has access
to the local machine.

```sh
kpt fn run DIR/ --image gcr.io/project/image:tag
```

Just like container functions, exec functions accept input as arguments after `--`

```sh
kpt fn run DIR/ --enable-exec --exec-path /path/to/executable -- foo=bar
```

## Declarative Run

Exec functions may also be run by declaring the function in a resource using the
`config.kubernetes.io/function` annotation.

To run the declared function use `kpt fn run DIR/ --enable-exec` on the directory containing
the example.

```yaml
apiVersion: example.com/v1beta1
kind: ExampleKind
metadata:
  name: function-input
  annotations:
    config.kubernetes.io/function: |
      exec:
        path: /path/to/executable
```

Note: if the `--enable-exec` flag is not provided, `kpt fn run DIR/` will ignore the exec
function and exit 0.

[kpt-functions-sdk]: https://github.com/GoogleContainerTools/kpt-functions-sdk