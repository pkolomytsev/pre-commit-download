# pre-commit-download

Custom PEP 517 compatible build-backend based on `setuptools.build_meta` for
downloading and installing single-file binaries during building wheel.

## Usage

All binary requirements must be defined in the `pyproject.toml`.

Schema:

<!-- markdownlint-disable MD013 -->

```toml
[build-system]
requires = [
    "pre-commit-download~=0.1.0",
]
build-backend = "pre_commit_download.hooks"

[tool.pre-commit-download]
install_root = "src/<module_name>/bin"  # optional, defaults to **/<module_name>

[tool.pre-commit-download.binaries]
"<executable-name>" = [
    {sys_platform = "<sys.platform>", platform_machine = "<platform.machine()>", extract_method = "<tarfile|zipfile>", extract_path = "<executable-name-v1.0.0/executable-name>", exec_suffix="<str>", url = "https://..."},
    {...},
]
"<executable-name>" = [
    {...},
]
```

<!-- markdownlint-enable MD013 -->

Required parameters:

- `<executable-name>` - the name of executable file(s) without extension
- `sys_platform` - compares to `sys.platform`
- `platform_machine` - compares to `platform.machine()`
- `url` - source URL

Optional parameters:

- `install_root` - a relative path from the project root there executables
  should be installed, default to the client's module name
- `exec_suffix` - optional, a platform specified executable suffix (like `exe`)
- `extract_method` - optional, an archive unpacking provider (`tarfile` or
  `zipfile`); not required if `url` points to unpacked file
- `extract_path` - optional, a relative path to the executable inside the
  archive, defaults to `<executable-name>` + `<exec_suffix>` (if defined);
  not required if `url` points to unpacked file
