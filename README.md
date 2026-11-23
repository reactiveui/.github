# .github

Org-level community health files and shared automation defaults for the [@reactiveui](https://github.com/reactiveui) organisation.

This repository provides:

- **Issue templates** — `.github/ISSUE_TEMPLATE/` (bug report, feature request, and contact links).
- **Pull request template** — `.github/PULL_REQUEST_TEMPLATE.md`.
- **Funding configuration** — `.github/FUNDING.yml`.
- **Shared workflows** — e.g. `.github/workflows/lock-threads.yml` for auto-locking stale issues and PRs.
- **Renovate defaults** — `renovate.json` with grouping rules for the .NET / NuGet ecosystem.

GitHub falls back to these files for any repository in the org that does not provide its own. See the [GitHub docs on default community health files](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file) for details.

## Renovate

Use the shared preset in your repository's Renovate configuration:

```json
{
  "extends": ["local>reactiveui/.github:renovate"]
}
```

The preset enables the NuGet manager for `.cs` files. Renovate can update versioned
`#:package` and `#:sdk` directives in apps run with `dotnet run --file`.
An unversioned directive uses central package management, so keep its version in
`Directory.Packages.props`.

The NuGet manager also handles project files, central package versions,
`VersionOverride` and .NET tool manifests. The preset explicitly enables updates
for versioned MSBuild SDK declarations, including `<Project Sdk="Uno.Sdk/7.0.22">`,
`<Sdk Name="Uno.Sdk" Version="7.0.22" />` and versioned `<Import Sdk="...">`
elements. Uno SDK updates join the Uno Platform package group.

The GitHub Actions manager handles action references and `actions/setup-dotnet`
version inputs.

For a tool version stored in an action or workflow input default, add an annotation:

```yaml
safesign-version:
  # renovate: datasource=nuget depName=SafeSign
  default: '2.1.0'
```

The preset also tracks annotated `_VERSION` values in workflow environment variables
and Dockerfile `ARG` or `ENV` instructions. Windows App Runtime input defaults keep
the `major.minor` format used by the installer.

Shared rules group related analyzers, build tools, test tools, adapters and package
families. Runtime packages stay in their .NET 10 or .NET 11 major band. Renovate uses
its default stable-release policy. Keep repository-specific compatibility pins in
that repository's configuration. Local rules apply after the shared rules.

See Renovate's [NuGet manager](https://docs.renovatebot.com/modules/manager/nuget/),
[custom manager presets](https://docs.renovatebot.com/presets-customManagers/) and
[configuration options](https://docs.renovatebot.com/configuration-options/).

## Contributing

Please read the [ReactiveUI Contribute guide](https://www.reactiveui.net/contribute/index.html) before opening an issue or PR.

## Sponsors

[JetBrains](https://www.jetbrains.com/) gives ReactiveUI's maintainers licences for its tools through its
[open source support programme](https://www.jetbrains.com/community/opensource/).
[Anthropic](https://www.anthropic.com/) supports them with [Claude](https://claude.com/) through
[Claude for Open Source](https://claude.com/contact-sales/claude-for-oss).
[OpenAI](https://openai.com/) supports them with [Codex](https://openai.com/codex/) through
[Codex for Open Source](https://developers.openai.com/community/codex-for-oss).
[Snyk](https://snyk.io/) gives the maintainers access to security tools through its
[Secure Developer Program](https://snyk.io/open-source/).

[![JetBrains](https://raw.githubusercontent.com/reactiveui/website/main/docs/images/sponsors/jetbrains.svg)](https://www.jetbrains.com/)
[![Claude by Anthropic](https://raw.githubusercontent.com/reactiveui/website/main/docs/images/sponsors/claude.svg)](https://claude.com/)
[![OpenAI](https://raw.githubusercontent.com/reactiveui/website/main/docs/images/sponsors/openai.svg)](https://openai.com/codex/)
[![Sponsored by Snyk](https://raw.githubusercontent.com/reactiveui/website/main/docs/images/sponsors/snyk.svg)](https://snyk.io/)

See [our sponsors](https://www.reactiveui.net/sponsors/) for more information.
JetBrains, Claude, Anthropic, OpenAI and Codex names and logos are trademarks of their respective owners.
Snyk and the Snyk logo are trademarks of Snyk Ltd.
