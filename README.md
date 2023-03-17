# bazel-zig-cc toolchain resolution issue

In trying to add bazel-zig-cc to prysmaticlabs/prysm in PR [12135](https://github.com/prysmaticlabs/prysm/pull/12135), I found that the zig toolchain was not chosen by bazel unless the extra_toolchains flag was being used.

## Reproduction

The issue can be seen via cquery.

```sh
bazel cquery 'somepath(//cmd:all, @zig_sdk//...)' 
# no paths found

bazel cquery 'somepath(//cmd:all, @zig_sdk//...)' --extra_toolchains @zig_sdk//toolchain:linux_amd64_gnu.2.31
# some path is found
```

If you comment out `llvm_register_toolchains()` in the WORKSPACE file, then the first command above returns somepath.
