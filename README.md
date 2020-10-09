# Ctags

Utilize Ctags with Textadept.

Install this module by copying it into your *~/.textadept/modules/* directory
or Textadept's *modules/* directory, and then putting the following in your
*~/.textadept/init.lua*:

    require('ctags')

There will be a "Search > Ctags" menu.

There are four ways to tell Textadept about *tags* files:

  1. Place a *tags* file in a project's root directory. This file will be
     used in a tag search from any of that project's source files.
  2. Add a *tags* file or list of *tags* files to the [`ctags`](#ctags) module for
     a project root key. This file(s) will be used in a tag search from any
     of that project's source files.
     For example: `ctags['/path/to/project'] = '/path/to/tags'`.
  3. Add a *tags* file to the [`ctags`](#ctags) module. This file will be used in
     any tag search.
     For example: `ctags[#ctags + 1] = '/path/to/tags'`.
  4. As a last resort, if no *tags* files were found, or if there is no match
     for a given symbol, a temporary *tags* file is generated for the current
     file and used.

Textadept will use any and all *tags* files based on the above rules.

## Generating Ctags and API Documentation

This module can also help generate Ctags files and API documentation files
that can be read by Textadept. This is typically configured per-project.
For example, a C project might want to generate tags and API documentation
for all files and subdirectories in a *src/* directory:

    ctags.ctags_flags['/path/to/project'] = '-R src/'
    table.insert(textadept.editing.api_files.ansi_c, '/path/to/project/api')

A Lua project has a couple of options for generating tags and API
documentation:

    -- Use ctags with some custom flags for improved Lua parsing.
    ctags.ctags_flags['/path/to/project'] = ctags.LUA_FLAGS
    table.insert(textadept.editing.api_files.lua, '/path/to/project/api')

    -- Use Textadept's tags and api generator, which depends on LuaDoc
    -- (https://keplerproject.github.io/luadoc/) being installed.
    ctags.ctags_flags['/path/to/project'] = ctags.LUA_GENERATOR
    ctags.api_commands['/path/to/project'] = ctags.LUA_GENERATOR
    table.insert(require('lua').tags, '/path/to/project/tags')
    table.insert(textadept.editing.api_files.lua, '/path/to/project/api')

Then, invoking Search > Ctags > Generate Project Tags and API menu item will
generate the tags and api files.

## Key Bindings

Windows, Linux, BSD|macOS|Terminal|Command
-------------------|-----|--------|-------
**Search**         |     |        |
F12                |F12  |F12     |Goto Ctag
Shift+F12          |⇧F12 |S-F12   |Goto Ctag...


## Fields defined by `ctags`

<a id="ctags.LUA_FLAGS"></a>
### `ctags.LUA_FLAGS` (string)

A set of command-line options for ctags that better parses Lua code.
  Combine this with other flags in [`ctags.ctags_flags`](#ctags.ctags_flags) if Lua files will
  be parsed.

<a id="ctags.LUA_GENERATOR"></a>
### `ctags.LUA_GENERATOR` (string)

Placeholder value that indicates Textadept's built-in Lua tags and api file
  generator should be used instead of ctags. Requires LuaDoc to be installed.

<a id="textadept.editing.autocompleters.ctag"></a>
### `textadept.editing.autocompleters.ctag` (function)

Autocompleter function for ctags. (Names only; not context-sensitive).

<a id="ctags.ctags"></a>
### `ctags.ctags` (string)

Path to the ctags executable.
  The default value is `'ctags'`.

<a id="ctags.generate_default_api"></a>
### `ctags.generate_default_api` (bool)

Whether or not to generate simple api documentation files based on *tags*
  file contents. For example, functions are documented with their signatures
  and source file paths.
  This *api* file is generated in the same directory as *tags* and can be
  read by `textadept.editing.show_documentation` as long as it was added
  to `textadept.editing.api_files` for a given language.
  The default value is `true`.


## Functions defined by `ctags`

<a id="ctags.goto_tag"></a>
### `ctags.goto_tag`(*tag*)

Jumps to the source of string *tag* or the source of the word under the
caret.
Prompts the user when multiple sources are found.

Parameters:

* *`tag`*: The tag to jump to the source of.


## Tables defined by `ctags`

<a id="ctags.api_commands"></a>
### `ctags.api_commands`

Map of project root paths to string commands, or functions that return such
strings, that generate an *api* file that Textadept can read via
`textadept.editing.show_documentation()`.
The user is responsible for adding the generated api file to
`textadept.editing.api_files[lexer]` for each lexer name the file applies to.

<a id="ctags.ctags_flags"></a>
### `ctags.ctags_flags`

Map of project root paths to string command-line options, or functions that
return such strings, that are passed to ctags when generating project tags.

See also:

* [`ctags.LUA_FLAGS`](#ctags.LUA_FLAGS)

---
