## Building

For C++ Visual Studio projects use the following.

**Locate MSBuild with `vswhere`, and build only with that.** Never a bare `msbuild`
off `PATH`, never `dotnet build`, never cmake.

```powershell
$vswhere = Join-Path ${env:ProgramFiles(x86)} 'Microsoft Visual Studio\Installer\vswhere.exe'
$msbuild = & $vswhere -version '[17.0,18.0)' -products * -requires Microsoft.Component.MSBuild Microsoft.VisualStudio.Component.VC.Tools.x86.x64 -find 'MSBuild\Current\Bin\amd64\MSBuild.exe' | Select-Object -First 1
& $msbuild {ProjectName}.sln /m /p:Configuration=Debug /p:Platform=x64 /verbosity:minimal /nologo
```

CMake may be used if the project is meant to be cross-platform.

For C# use the `dotnet` command.

- For C++ **Always build the full solution.** `$ProjectName.sln`, x64 only — never an individual
  `.vcxproj`, and no `/t:<project>` shortcut to build one project faster. The projects may
  depend on `$(SolutionDir)`
- For either, the output lands in `bin/<config>/`, intermediates in `.build/<config>/`.

## Git

- If a new project is started and no git repository exists yet, create one.
- **Atomic commits, one responsibility each.** Split unrelated changes.
- **Single-line messages** in the form `Component: Short description of the change` or `Feature: Short description of the change`.
  Look at `git log` for the established component names.
- **Fixes get squashed where they belong.** A defect in a commit that is still on the
  branch is amended or `--fixup`'d into that commit — never a follow-up
  "fix xyz" commit.
- Never `stash`, `reset`, or `checkout` the working tree — the user may have unrelated
  work in progress. `git add` only the files you touched.

## C++ conventions

Tabs at width 4, Allman braces, 120 columns, LF, final newline.

- **Always brace control flow**, even a single-line body.
- **No C-style casts.** `static_cast` / `reinterpret_cast`, explicitly.
- **No `[[nodiscard]]`.**
- **No trailing or whitespace-only lines.**
- **No `#pragma comment(lib, ...)`.** This may be a Visual Studio project — add the reference or the linker input to the `.vcxproj`.
- **No static classes or free-floating globals.** State hangs off a `project_name_t` or off something `project_name_t` owns.
- **Don't repeat yourself.** Factor shared behaviour out instead of copying it.
- **Comments stay short.** One line on the non-obvious *why*. No paragraphs of
  justification unless a future reader genuinely cannot recover the reasoning.

## Project AGENTS.md

If a project does not yet have an AGENTS.md, please copy the rules from this project into a fresh AGENTS.md so other users have the same rules applied.
Keep AGENTS.md up-to-date in any project.

## Working with the user

Decide for yourself when intent is already established; ask only when a different
reading would produce materially different work.
