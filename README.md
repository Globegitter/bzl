# bzl

`bzl` is a command-line application that makes it easier to work with multiple
versions of bazel.  You can install different versions and easily switch between
them.

`bzl` is a drop-in replacement for `bazel`. Any commands not recognized by `bzl`
are passed through as-is to `bazel`.

## How is `bzl` pronounced?

`bzl` is pronounced like bezel, as in "*the bezel of a watch*". The name invokes
it's function (a wrapper around bazel).

> In the 1950s, watchmakers realized that an external bezel was the best way to
> add functions to a watch without complicating the movement, and so the
> external watch bezel was born.

## Install bzl

`bzl` ships as a single executable go binary. Download the file directly from
the [Github Releases Page](https://github.com/bzl-io/bzl/releases) for the
precompiled platform of your choice (or build from source).

Once downloaded `chmod +x` and `mv bzl ~/bin/bazel` and type `bazel install` to
list the available versions.

Specify the version of bazel to use either via an environment variable or
command line flag (example: `BAZEL_VERSION=0.24.1`; `--bazel=0.19.2`).

## `bzl` commands

### `$ bazel --help`

Show help.

### `$ bazel install`

List or install available bazel installs.

Examples:

| Command | Description |
| --- | --- |
| `$ bazel install` | List all available releases |
| `$ bazel install 0.24.1` | Install bazel release 0.24.1 |
| `$ bazel install --assets 0.24.1` | Show the assets bundled in install 0.24.1 |

To build bazel from source, specify a commit-id:

```
$ bazel install 9a32b861799278217c37eb32f864a951ca99617e
(( bzl )) Install successful.  Remember to 'export BAZEL_VERSION=9a32b861799278217c37eb32f864a951ca99617e'
```

> The command above will clone bazel to
> `$HOME/.cache/bzl/github.com/bazebuild/bazel`.  If you already have a version
> of bazel installed that you'd rather use instead, specify
> `--bazel_source_dir`. If you want to build & install from `bazel_source_dir`
> directly (without changing to a specific commit), use `bazel install snapshot
> --bazel_source_dir=/path/to/bazel` ().

### `$ bazel use`

Print a repository rule for a github bazel repository.

Without a release tag, list available releases:

```
$ bazel use grpc-ecosystem/grpc-gateway 

v1.8.5        Fri Mar 15 2019
v1.8.4        Wed Mar 13 2019
v1.8.3        Mon Mar 11 2019
v1.8.2        Thu Mar 07 2019
v1.8.1        Sat Mar 02 2019
```

With a release tag, output an `http_archive` rule:

```
$ bazel use grpc-ecosystem/grpc-gateway v1.8.5

http_archive(
    name = "grpc_ecosystem_grpc_gateway",
    urls = ["https://github.com/grpc-ecosystem/grpc-gateway/archive/v1.8.5.tar.gz"],
    strip_prefix = "grpc-gateway-1.8.5",
    sha256 = "9d7cf2ce799002024f215d3ff2df4882c347563478093a4671b13154ba37982c",
)
```

The `bazelbuild` organization is assumed if you leave out the organization name:

```
$ bazel use rules_go

0.18.3    Fri Apr 12 2019
0.17.4    Fri Apr 12 2019
0.16.10   Fri Apr 12 2019
0.18.2    Sat Apr 06 2019
...
```

You can also specify a commit-id:

```
$ bazel use rules_go ef7cca8857f2f3a905b86c264737d0043c6a766e

http_archive(
    name = "io_bazel_rules_go",
    urls = ["https://github.com/bazelbuild/rules_go/archive/ef7cca8857f2f3a905b86c264737d0043c6a766e.tar.gz"],
    strip_prefix = "rules_go-ef7cca8857f2f3a905b86c264737d0043c6a766e",
    sha256 = "1a400dc2f69971e3ebce29f7950dc38f8bb7e41c727258b6d2fb70060a4ce429",
)
```

To list recent commits on a particular branch:

```
$ bazel use rules_go --history=master
(( bzl )) 2019/04/29 06:36:48 Listing recent commit history for bazelbuild/rules_go...
ef7cca8857f2f3a905b86c264737d0043c6a766e   Wed Apr 24 2019   bazel_test: fix load of generate_toolchain_names (#2040)
56ecf4406adfd8917ac59ddd0ea008ff1d8f722a   Wed Apr 24 2019   Ensure bazel paths don't leak in stdlib builds (#1945)
df1bb575ad1b35703e5b62fd0455907896c9eb32   Tue Apr 23 2019   update pin to bazel toolchains repo (#2038)
528f6faf83f85c23da367d61f784893d1b3bd72b   Sat Apr 20 2019   GoCompilePkg: unified action for building a Go package (#2027)
...
```

### `$ bazel target`

Pretty-print available targets in the current workspace.

Example:

```
$ bazel targets

go_library        //proto/bes:go_default_library
go_library        //:go_default_library
go_library        //command:go_default_library
go_library        //command/targets:go_default_library
_gazelle_runner   //:gazelle-runner
go_library        //gh:go_default_library
go_proto_library  //proto/bes:build_event_stream_go_proto
proto_library     //proto/build:bzl_proto
proto_library     //proto/bes:build_event_stream_proto
go_proto_library  //proto/build:bzl_go_proto
go_library        //command/release:go_default_library
sh_binary         //:gazelle
go_library        //command/install:go_default_library
go_library        //config:go_default_library
go_test           //:go_default_test
go_library        //proto/build:go_default_library
go_library        //bazelutil:go_default_library
go_binary         //:bzl
go_library        //command/use:go_default_library
_buildifier       //:buildifier
```

This command is essentially a synonym for `bazel query` with formatted output.

Additional example:

```
$  bazel target 'deps(//:*)' --sort=kind --align pkg --include go_bin
go_binary                                                            //:bzl
go_binary   @bazel_gazelle//language/go/gen_std_package_list:gen_std_package_list
go_binary                 @bazel_gazelle//language/proto/gen:gen_known_imports
go_binary      @com_github_bazelbuild_buildtools//buildifier:buildifier
go_binary  @com_github_bazelbuild_buildtools//generatetables:generatetables
go_binary         @com_github_golang_protobuf//protoc-gen-go:protoc-gen-go
go_binary  @com_google_bazelbuild_buildtools//generatetables:generatetables
go_binary                    @org_golang_x_tools//cmd/goyacc:goyacc
```


### `$ bazel release`

Publish a release for a `go_binary` target.

This is a simple variant of the "goreleaser" tool for go binaries built by
rules_go.  Here's what the command does:

1. Reads the name of a `go_binary` target (example: `//:bzl`) as the first
   command line argument.
2. Takes a list of platform names via the `--platform` argument (example: `linux_amd64`)
   it runs the equivalent of `bazel build --platforms @io_bazel_rules_go//go/toolchain:linux_amd64`
3. Copies the output file to `{ASSET_DIR}/{LABEL_NAME}-{TAG}-{PLATFORM_NAME}`
   (example: `.assets/bzl-v0.1.3-linux-x64_64`) where `--asset_dir={ASSET_DIR}`,
   `--tag={TAG}`, and `--platform={PLATFORM}`.  The `{PLATFORM_NAME}` can be
   remapped to a different string via `--platform_name={PLATFORM}=NAME`
   (example: `windows_amd64=windows-x64_64`).
4. Reads a release notes file (example: `--notes=RELEASE.md`).
5. Uploads the staged assets to github.  Authentication is via environment
   variables (example: `BZL_GH_USERNAME=pcj`
   `BZL_GH_PASSWORD={PERSONAL_ACCESS_TOKEN}`)
6. Tags the release as `--tag={TAG}`. 

Example:

```
$ bazel release \
    --owner=bzl-io \
    --repo=bzl \
    --commit=91801a92ea21cd73471e5a83ad2519d1a3f257f0 \
    --tag=v0.1.3 \
    --notes=RELEASE.md \
    --platform=linux_amd64 \
    --platform=darwin_amd64 \
    --platform=windows_amd64 \
    //:bzl
```

### `$ bazel lint`

Print linting issues in build files (buildifier).

### `$ bazel fmt`

Fix formatting issues in build files (buildifier).

### `$ bazel exec`

Similar to `bazel run` but without running in the sandbox, useful when you don't
want to use `bazel run` and may not be able to easily predict the name of the
output binary.

For example, the following are largely equivalent:

```
$ bazel build //:bzl && ./bazel-bin/linux_amd64_stripped/bzl
$ bazel exec //:bzl
```
