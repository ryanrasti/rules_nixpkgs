<!-- Generated with Stardoc: http://skydoc.bazel.build -->

<!-- Edit the docstring in `toolchains/cc/cc.bzl` and run `bazel run //docs:update-README.md` to change this repository's `README.md`. -->

Rules for importing a C++ toolchain from Nixpkgs.

## Rules

* [nixpkgs_cc_configure](#nixpkgs_cc_configure)


# Reference documentation

<a id="#nixpkgs_cc_configure"></a>

### nixpkgs_cc_configure

<pre>
nixpkgs_cc_configure(<a href="#nixpkgs_cc_configure-name">name</a>, <a href="#nixpkgs_cc_configure-attribute_path">attribute_path</a>, <a href="#nixpkgs_cc_configure-nix_file">nix_file</a>, <a href="#nixpkgs_cc_configure-nix_file_content">nix_file_content</a>, <a href="#nixpkgs_cc_configure-nix_file_deps">nix_file_deps</a>, <a href="#nixpkgs_cc_configure-repositories">repositories</a>,
                     <a href="#nixpkgs_cc_configure-repository">repository</a>, <a href="#nixpkgs_cc_configure-nixopts">nixopts</a>, <a href="#nixpkgs_cc_configure-quiet">quiet</a>, <a href="#nixpkgs_cc_configure-fail_not_supported">fail_not_supported</a>, <a href="#nixpkgs_cc_configure-exec_constraints">exec_constraints</a>,
                     <a href="#nixpkgs_cc_configure-target_constraints">target_constraints</a>, <a href="#nixpkgs_cc_configure-register">register</a>)
</pre>

Use a CC toolchain from Nixpkgs. No-op if not a nix-based platform.

By default, Bazel auto-configures a CC toolchain from commands (e.g.
`gcc`) available in the environment. To make builds more hermetic, use
this rule to specify explicitly which commands the toolchain should use.

Specifically, it builds a Nix derivation that provides the CC toolchain
tools in the `bin/` path and constructs a CC toolchain that uses those
tools. Tools that aren't found are replaced by `${coreutils}/bin/false`.
You can inspect the resulting `@<name>_info//:CC_TOOLCHAIN_INFO` to see
which tools were discovered.

If you specify the `nix_file` or `nix_file_content` argument, the CC
toolchain is discovered by evaluating the corresponding expression. In
addition, you may use the `attribute_path` argument to select an attribute
from the result of the expression to use as the CC toolchain (see example below).

If neither the `nix_file` nor `nix_file_content` argument is used, the
toolchain is discovered from the `stdenv.cc` and the `stdenv.cc.bintools`
attributes of the given `<nixpkgs>` repository.

```
# use GCC 11
nixpkgs_cc_configure(
  repository = "@nixpkgs",
  nix_file_content = "(import <nixpkgs> {}).gcc11",
)
```
```
# use GCC 11 (same result as above)
nixpkgs_cc_configure(
  repository = "@nixpkgs",
  attribute_path = "gcc11",
  nix_file_content = "import <nixpkgs> {}",
)
```
```
# use the `stdenv.cc` compiler (the default of the given @nixpkgs repository)
nixpkgs_cc_configure(
  repository = "@nixpkgs",
)
```

This rule depends on [`rules_cc`](https://github.com/bazelbuild/rules_cc).

**Note:**
You need to configure `--crosstool_top=@<name>//:toolchain` to activate
this toolchain.


#### Parameters

<table class="params-table">
<colgroup>
<col class="col-param" />
<col class="col-description" />
</colgroup>
<tbody>
<tr id="nixpkgs_cc_configure-name">
<td><code>name</code></td>
<td>

optional.
default is <code>"local_config_cc"</code>

</td>
</tr>
<tr id="nixpkgs_cc_configure-attribute_path">
<td><code>attribute_path</code></td>
<td>

optional.
default is <code>""</code>

<p>

optional, string, Obtain the toolchain from the Nix expression under this attribute path. Requires `nix_file` or `nix_file_content`.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-nix_file">
<td><code>nix_file</code></td>
<td>

optional.
default is <code>None</code>

<p>

optional, Label, Obtain the toolchain from the Nix expression defined in this file. Specify only one of `nix_file` or `nix_file_content`.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-nix_file_content">
<td><code>nix_file_content</code></td>
<td>

optional.
default is <code>""</code>

<p>

optional, string, Obtain the toolchain from the given Nix expression. Specify only one of `nix_file` or `nix_file_content`.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-nix_file_deps">
<td><code>nix_file_deps</code></td>
<td>

optional.
default is <code>[]</code>

<p>

optional, list of Label, Additional files that the Nix expression depends on.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-repositories">
<td><code>repositories</code></td>
<td>

optional.
default is <code>{}</code>

<p>

dict of Label to string, Provides `<nixpkgs>` and other repositories. Specify one of `repositories` or `repository`.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-repository">
<td><code>repository</code></td>
<td>

optional.
default is <code>None</code>

<p>

Label, Provides `<nixpkgs>`. Specify one of `repositories` or `repository`.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-nixopts">
<td><code>nixopts</code></td>
<td>

optional.
default is <code>[]</code>

<p>

optional, list of string, Extra flags to pass when calling Nix. Subject to location expansion, any instance of `$(location LABEL)` will be replaced by the path to the file ferenced by `LABEL` relative to the workspace root.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-quiet">
<td><code>quiet</code></td>
<td>

optional.
default is <code>False</code>

<p>

bool, Whether to hide `nix-build` output.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-fail_not_supported">
<td><code>fail_not_supported</code></td>
<td>

optional.
default is <code>True</code>

<p>

bool, Whether to fail if `nix-build` is not available.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-exec_constraints">
<td><code>exec_constraints</code></td>
<td>

optional.
default is <code>None</code>

<p>

Constraints for the execution platform.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-target_constraints">
<td><code>target_constraints</code></td>
<td>

optional.
default is <code>None</code>

<p>

Constraints for the target platform.

</p>
</td>
</tr>
<tr id="nixpkgs_cc_configure-register">
<td><code>register</code></td>
<td>

optional.
default is <code>True</code>

<p>

bool, enabled by default, Whether to register (with `register_toolchains`) the generated toolchain and install it as the default cc_toolchain.

</p>
</td>
</tr>
</tbody>
</table>

