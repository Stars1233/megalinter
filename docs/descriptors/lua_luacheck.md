---
title: luacheck configuration in MegaLinter
description: How to use luacheck (configure, ignore files, ignore errors, help & version documentations) to analyze LUA files
---
<!-- markdownlint-disable MD033 MD041 -->
<!-- @generated by .automation/build.py, please don't update manually -->
# luacheck
[![GitHub stars](https://img.shields.io/github/stars/lunarmodules/luacheck?cacheSeconds=3600)](https://github.com/lunarmodules/luacheck) [![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/lunarmodules/luacheck?sort=semver)](https://github.com/lunarmodules/luacheck/releases) [![GitHub last commit](https://img.shields.io/github/last-commit/lunarmodules/luacheck)](https://github.com/lunarmodules/luacheck/commits) [![GitHub commit activity](https://img.shields.io/github/commit-activity/y/lunarmodules/luacheck)](https://github.com/lunarmodules/luacheck/graphs/commit-activity/) [![GitHub contributors](https://img.shields.io/github/contributors/lunarmodules/luacheck)](https://github.com/lunarmodules/luacheck/graphs/contributors/)

**luacheck** is a static analyzer and linter for Lua code that detects various issues such as usage of undefined global variables, unused variables and values, accessing uninitialized variables, and unreachable code. It provides comprehensive code quality analysis for Lua projects.

**Key Features:**

- **Multi-version Lua support** for Lua 5.1-5.4 and LuaJIT syntax checking
- **Undefined global variable detection** to catch common runtime errors
- **Unused variable and value analysis** to improve code cleanliness
- **Uninitialized variable access detection** for safer code
- **Unreachable code identification** to optimize program flow
- **Custom project globals** configuration for specific environments
- **Standard library version selection** matching your Lua version
- **Warning filtering** by type and variable name patterns
- **Inline configuration comments** for granular control
- **Project-wide config files** using `.luacheckrc`
- **Detailed error reporting** with line numbers and suggestions

## luacheck documentation

- Version in MegaLinter: **1.2.0**
- Visit [Official Web Site](https://luacheck.readthedocs.io){target=_blank}
- See [How to configure luacheck rules](https://luacheck.readthedocs.io/en/stable/config.html){target=_blank}
  - If custom `.luacheckrc` config file isn't found, [.luacheckrc](https://github.com/oxsecurity/megalinter/tree/main/TEMPLATES/.luacheckrc){target=_blank} will be used
- See [How to disable luacheck rules in files](https://luacheck.readthedocs.io/en/stable/inline.html){target=_blank}
- See [Index of problems detected by luacheck](https://luacheck.readthedocs.io/en/stable/warnings.html){target=_blank}

[![luacheck - GitHub](https://gh-card.dev/repos/lunarmodules/luacheck.svg?fullname=)](https://github.com/lunarmodules/luacheck){target=_blank}

## Configuration in MegaLinter

- Enable luacheck by adding `LUA_LUACHECK` in [ENABLE_LINTERS variable](https://megalinter.io/beta/configuration/#activation-and-deactivation)
- Disable luacheck by adding `LUA_LUACHECK` in [DISABLE_LINTERS variable](https://megalinter.io/beta/configuration/#activation-and-deactivation)

| Variable                                 | Description                                                                                                                                                                                  | Default value                                   |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|
| LUA_LUACHECK_ARGUMENTS                   | User custom arguments to add in linter CLI call<br/>Ex: `-s --foo "bar"`                                                                                                                     |                                                 |
| LUA_LUACHECK_COMMAND_REMOVE_ARGUMENTS    | User custom arguments to remove from command line before calling the linter<br/>Ex: `-s --foo "bar"`                                                                                         |                                                 |
| LUA_LUACHECK_FILTER_REGEX_INCLUDE        | Custom regex including filter<br/>Ex: `(src\|lib)`                                                                                                                                           | Include every file                              |
| LUA_LUACHECK_FILTER_REGEX_EXCLUDE        | Custom regex excluding filter<br/>Ex: `(test\|examples)`                                                                                                                                     | Exclude no file                                 |
| LUA_LUACHECK_CLI_LINT_MODE               | Override default CLI lint mode<br/>- `file`: Calls the linter for each file<br/>- `project`: Call the linter from the root of the project                                                    | `file`                                          |
| LUA_LUACHECK_FILE_EXTENSIONS             | Allowed file extensions. `"*"` matches any extension, `""` matches empty extension. Empty list excludes all files<br/>Ex: `[".py", ""]`                                                      | `[".lua"]`                                      |
| LUA_LUACHECK_FILE_NAMES_REGEX            | File name regex filters. Regular expression list for filtering files by their base names using regex full match. Empty list includes all files<br/>Ex: `["Dockerfile(-.+)?", "Jenkinsfile"]` | Include every file                              |
| LUA_LUACHECK_PRE_COMMANDS                | List of bash commands to run before the linter                                                                                                                                               | None                                            |
| LUA_LUACHECK_POST_COMMANDS               | List of bash commands to run after the linter                                                                                                                                                | None                                            |
| LUA_LUACHECK_UNSECURED_ENV_VARIABLES     | List of env variables explicitly not filtered before calling LUA_LUACHECK and its pre/post commands                                                                                          | None                                            |
| LUA_LUACHECK_CONFIG_FILE                 | luacheck configuration file name</br>Use `LINTER_DEFAULT` to let the linter find it                                                                                                          | `.luacheckrc`                                   |
| LUA_LUACHECK_RULES_PATH                  | Path where to find linter configuration file                                                                                                                                                 | Workspace folder, then MegaLinter default rules |
| LUA_LUACHECK_DISABLE_ERRORS              | Run linter but consider errors as warnings                                                                                                                                                   | `false`                                         |
| LUA_LUACHECK_DISABLE_ERRORS_IF_LESS_THAN | Maximum number of errors allowed                                                                                                                                                             | `0`                                             |
| LUA_LUACHECK_CLI_EXECUTABLE              | Override CLI executable                                                                                                                                                                      | `['luacheck']`                                  |

## IDE Integration

Use luacheck in your favorite IDE to catch errors before MegaLinter !

|                                                                   <!-- -->                                                                    | IDE                                                  | Extension Name                                                                                  |                                                                                    Install                                                                                    |
|:---------------------------------------------------------------------------------------------------------------------------------------------:|------------------------------------------------------|-------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| <img src="https://github.com/oxsecurity/megalinter/raw/main/docs/assets/icons/brackets.ico" alt="" height="32px" class="megalinter-icon"></a> | [Brackets](https://brackets.io/)                     | [brackets-luacheck](https://github.com/Malcolm3141/brackets-luacheck)                           |                                               [Visit Web Site](https://github.com/Malcolm3141/brackets-luacheck){target=_blank}                                               |
|  <img src="https://github.com/oxsecurity/megalinter/raw/main/docs/assets/icons/emacs.ico" alt="" height="32px" class="megalinter-icon"></a>   | [Emacs](https://www.gnu.org/software/emacs/)         | [flycheck](http://www.flycheck.org/en/latest/languages.html#lua)                                |                                             [Visit Web Site](http://www.flycheck.org/en/latest/languages.html#lua){target=_blank}                                             |
| <img src="https://github.com/oxsecurity/megalinter/raw/main/docs/assets/icons/sublime.ico" alt="" height="32px" class="megalinter-icon"></a>  | [Sublime Text](https://www.sublimetext.com/)         | [SublimeLinter-luacheck](https://packagecontrol.io/packages/SublimeLinter-luacheck)             |                                          [Visit Web Site](https://packagecontrol.io/packages/SublimeLinter-luacheck){target=_blank}                                           |
|   <img src="https://github.com/oxsecurity/megalinter/raw/main/docs/assets/icons/vim.ico" alt="" height="32px" class="megalinter-icon"></a>    | [vim](https://www.vim.org/)                          | [Syntastic](https://github.com/vim-syntastic/syntastic/wiki/Lua%3A---luacheck)                  |                                      [Visit Web Site](https://github.com/vim-syntastic/syntastic/wiki/Lua%3A---luacheck){target=_blank}                                       |
|  <img src="https://github.com/oxsecurity/megalinter/raw/main/docs/assets/icons/vscode.ico" alt="" height="32px" class="megalinter-icon"></a>  | [Visual Studio Code](https://code.visualstudio.com/) | [vscode-luacheck](https://marketplace.visualstudio.com/items?itemName=dwenegar.vscode-luacheck) | [![Install in VSCode](https://github.com/oxsecurity/megalinter/raw/main/docs/assets/images/btn_install_vscode.png)](vscode:extension/dwenegar.vscode-luacheck){target=_blank} |

## MegaLinter Flavors

This linter is available in the following flavors

|                                                                         <!-- -->                                                                         | Flavor                                               | Description               | Embedded linters |                                                                                                                                                                       Info |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------------------------------------|:--------------------------|:----------------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| <img src="https://github.com/oxsecurity/megalinter/raw/main/docs/assets/images/mega-linter-square.png" alt="" height="32px" class="megalinter-icon"></a> | [all](https://megalinter.io/beta/supported-linters/) | Default MegaLinter Flavor |       126        | ![Docker Image Size (tag)](https://img.shields.io/docker/image-size/oxsecurity/megalinter/beta) ![Docker Pulls](https://img.shields.io/docker/pulls/oxsecurity/megalinter) |

## Behind the scenes

### How are identified applicable files

- File extensions: `.lua`

<!-- markdownlint-disable -->
<!-- /* cSpell:disable */ -->
### How the linting is performed

- luacheck is called one time by identified file (`file` CLI lint mode)

### Example calls

```shell
luacheck myfile.lua
```

```shell
luacheck --config .chktexrc myfile.lua
```


### Help content

```shell
Usage: luacheck ([--config <config>] | [--no-config])
       ([--default-config <default_config>] | [--no-default-config])
       [-h] [-g] [-u] [-r] [-a] [-s] [--no-self] [--std <std>] [-c]
       [-d] [-t] [-m] [--max-line-length <length>]
       [--no-max-line-length] [--max-code-line-length <length>]
       [--no-max-code-line-length] [--max-string-line-length <length>]
       [--no-max-string-line-length]
       [--max-comment-line-length <length>]
       [--no-max-comment-line-length]
       [--max-cyclomatic-complexity <complexity>]
       [--no-max-cyclomatic-complexity] [--filename <filename>]
       [-j <jobs>] [--formatter <formatter>] [-q] [--codes] [--ranges]
       [--no-color] [-v] <file> [<file>] ...
       ([--cache [<cache>]] | [--no-cache])
       [--ignore <patt> [<patt>] ...] [--enable <patt> [<patt>] ...]
       [--only <patt> [<patt>] ...] [--operators <patt> [<patt>] ...]
       [--globals [<name>] ...] [--read-globals [<name>] ...]
       [--new-globals [<name>] ...] [--new-read-globals [<name>] ...]
       [--not-globals [<name>] ...]
       [--exclude-files <glob> [<glob>] ...]
       [--include-files <glob> [<glob>] ...]

luacheck 1.2.0, a linter and a static analyzer for Lua.

Arguments:
   files                 List of files, directories and rockspecs to check. Pass
                         '-' to check stdin.

Options for filtering warnings:
   -g, --no-global       Filter out warnings related to global variables.
                         Equivalent to --ignore 1.
   -u, --no-unused       Filter out warnings related to unused variables and
                         values. Equivalent to --ignore [23].
   -r, --no-redefined    Filter out warnings related to redefined variables.
                         Equivalent to --ignore 4.
   -a, --no-unused-args  Filter out warnings related to unused arguments and
                         loop variables. Equivalent to --ignore 21[23].
   -s, --no-unused-secondaries
                         Filter out warnings related to unused variables set
                         together with used ones.
   --no-self             Filter out warnings related to implicit self argument.
   --ignore <patt> [<patt>] ...,
         -i <patt> [<patt>] ...
                         Filter out warnings matching these patterns.
                         If a pattern contains slash, part before slash matches
                         warning code and part after it matches name of related
                         variable. Otherwise, if the pattern contains letters or
                         underscore, it matches name of related variable.
                         Otherwise, the pattern matches warning code.
   --enable <patt> [<patt>] ...,
         -e <patt> [<patt>] ...
                         Do not filter out warnings matching these patterns.
   --only <patt> [<patt>] ...,
       -o <patt> [<patt>] ...
                         Filter out warnings not matching these patterns.
   --operators <patt> [<patt>] ...
                         Allow compound operators matching patterns

Options for configuring allowed globals:
   --std <std>           Set standard globals, default is max. <std> can be one
                         of:
                            max - union of globals of Lua 5.1, Lua 5.2, Lua 5.3
                            and LuaJIT 2.x;
                            min - intersection of globals of Lua 5.1, Lua 5.2,
                            Lua 5.3 and LuaJIT 2.x;
                            lua51 - globals of Lua 5.1 without deprecated ones;
                            lua51c - globals of Lua 5.1;
                            lua52 - globals of Lua 5.2;
                            lua52c - globals of Lua 5.2 with LUA_COMPAT_ALL;
                            lua53 - globals of Lua 5.3;
                            lua53c - globals of Lua 5.3 with LUA_COMPAT_5_2;
                            lua54 - globals of Lua 5.4;
                            lua54c - globals of Lua 5.4 with LUA_COMPAT_5_3;
                            luajit - globals of LuaJIT 2.x;
                            ngx_lua - globals of Openresty lua-nginx-module
                            0.10.10, including standard LuaJIT 2.x globals;
                            love - globals added by LÖVE;
                            minetest - globals added by minetest;
                            playdate - globals added by the Playdate SDK;
                            busted - globals added by Busted 2.0, by default
                            added for files ending with _spec.lua within spec,
                            test, and tests subdirectories;
                            rockspec - globals allowed in rockspecs, by default
                            added for files ending with .rockspec;
                            luacheckrc - globals allowed in Luacheck configs, by
                            default added for files ending with .luacheckrc;
                            none - no standard globals.

                         Sets can be combined using '+'. Extra sets can be
                         defined in config by adding to `stds` global in config.
   -c, --compat          Equivalent to --std max.
   --globals [<name>] ...
                         Add custom global variables (e.g. foo) or fields (e.g.
                         foo.bar) on top of standard ones.
   --read-globals [<name>] ...
                         Add read-only global variables or fields.
   --new-globals [<name>] ...
                         Set custom global variables or fields. Removes custom
                         globals added previously.
   --new-read-globals [<name>] ...
                         Set read-only global variables or fields. Removes
                         read-only globals added previously.
   --not-globals [<name>] ...
                         Remove custom and standard global variables or fields.
   -d, --allow-defined   Allow defining globals implicitly by setting them.
   -t, --allow-defined-top
                         Allow defining globals implicitly by setting them in
                         the top level scope.
   -m, --module          Limit visibility of implicitly defined globals to their
                         files.

Options for configuring line length limits:
   --max-line-length <length>
                         Set maximum allowed line length (default: 120).
   --no-max-line-length  Do not limit line length.
   --max-code-line-length <length>
                         Set maximum allowed length for lines ending with code
                         (default: 120).
   --no-max-code-line-length
                         Do not limit code line length.
   --max-string-line-length <length>
                         Set maximum allowed length for lines within a string
                         (default: 120).
   --no-max-string-line-length
                         Do not limit string line length.
   --max-comment-line-length <length>
                         Set maximum allowed length for comment lines (default:
                         120).
   --no-max-comment-line-length
                         Do not limit comment line length.

Configuration file options:
   --config <config>     Path to configuration file. (default: .luacheckrc)
   --no-config           Do not look up configuration file.
   --default-config <default_config>
                         Path to configuration file to use if --[no-]config is
                         not used and project-specific .luacheckrc is not found.
                         (default: /root/.config/luacheck/.luacheckrc)
   --no-default-config   Do not use default configuration file.

File filtering options:
   --exclude-files <glob> [<glob>] ...
                         Do not check files matching these globbing patterns.
   --include-files <glob> [<glob>] ...
                         Do not check files not matching these globbing
                         patterns.

Performance optimization options:
   --cache [<cache>]     Path to cache directory. (default:
                         /root/.cache/luacheck)
   --no-cache            Do not use cache.
       -j <jobs>,        Check <jobs> files in parallel (default: 1).
   --jobs <jobs>         Warning: LuaLanes not found, parallel checking
                         disabled.

Output formatting options:
   --formatter <formatter>
                         Use custom formatter. <formatter> must be a module name
                         or one of:
                            TAP - Test Anything Protocol formatter;
                            JUnit - JUnit XML formatter;
                            visual_studio - MSBuild/Visual Studio aware
                            formatter;
                            plain - simple warning-per-line formatter;
                            default - standard formatter.
   -q, --quiet           Suppress output for files without warnings.
                         -qq: Suppress output of warnings.
                         -qqq: Only print total number of warnings and errors.
   --codes               Show warning codes.
   --ranges              Show ranges of columns related to warnings.
   --no-color            Do not color output.

Other options:
   -h, --help            Show this help message and exit.
   --max-cyclomatic-complexity <complexity>
                         Set maximum cyclomatic complexity for functions.
   --no-max-cyclomatic-complexity
                         Do not limit function cyclomatic complexity (default).
   --filename <filename> Use another filename in output and for selecting
                         configuration overrides.
   -v, --version         Show version info and exit.

Links:

   Luacheck on GitHub: https://github.com/lunarmodules/luacheck
   Luacheck documentation: https://luacheck.readthedocs.org
```

### Installation on mega-linter Docker image

- Dockerfile commands :
```dockerfile
# Parent descriptor install
RUN wget --tries=5 https://www.lua.org/ftp/lua-5.3.5.tar.gz -O - -q | tar -xzf - \
    && cd lua-5.3.5 \
    && make linux \
    && make install \
    && cd .. && rm -r lua-5.3.5/

# Linter install
# renovate: datasource=github-tags depName=cvega/luarocks
ARG LUA_LUACHECK_VERSION=3.3.1

RUN wget --tries=5 https://github.com/cvega/luarocks/archive/v${LUA_LUACHECK_VERSION}-super-linter.tar.gz -O - -q | tar -xzf - \
    && cd luarocks-${LUA_LUACHECK_VERSION}-super-linter \
    && ./configure --with-lua-include=/usr/local/include \
    && make \
    && make -b install \
    && cd .. && rm -r luarocks-${LUA_LUACHECK_VERSION}-super-linter/ \
    && luarocks install luacheck \
    && cd /

```

- APK packages (Linux):
  - [readline-dev](https://pkgs.alpinelinux.org/packages?branch=v3.21&arch=x86_64&name=readline-dev)
  - [openssl](https://pkgs.alpinelinux.org/packages?branch=v3.21&arch=x86_64&name=openssl)
