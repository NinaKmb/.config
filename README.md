# dotfiles

A collection of my personal configuration for system elements and my software of choice, meant to be added as a submodule under development repos.
**Windows machines need to have git configured in order to be able to handle symlinks!**

## Usage

For development and common standards symlink to the root of your repo the following files:

- .editorconfig
- .gitignore
- .Directory.Build.props
- .Directory.Build.targets

Additionally, symlink _Resharper.sln.DotSettings_ next to your solution file, and replace _Resharper_ on the link target filename with the name of your solution file.

In order to avoid duplicate PackageReference definitions and unpredictable behavior, the following nugets should be handled by [Directory.Build.props][buildProps] **only**:

- coverlet.collector
- coverlet.msbuild
- Microsoft.CodeAnalysis.FxCopAnalyzers
- Microsoft.SourceLink.Bitbucket.Git
- Microsoft.SourceLink.GitHub
- Microsoft.SourceLink.GitLab
- SerilogAnalyzer
- SmartanAlyzers.ExceptionAnalyzer
- SmartAnalyzers.MultithreadingAnalyzer
- StyleCop.Analyzers

To update them, tick their version numbers in the [Directory.Build.props][buildProps] file and reload the solution.

## Azure DevOps integration

The [azure-pipelines-dotnet.yml][pipeline] file is preconfigured for dotnet projects with the following services:

- SonarCloud
- Coveralls
- CodeCov
- Nuget.org publishing

In order to make it work, copy it to the root of your project as ```azure-pipelines.yml```, replace the value of ```SONAR_PROJECT``` at the pipeline variables section and make sure your pipeline has the following variables set on the DevOps portal:

- GitHubApiKey: GitHub access token
- SonarCloudOrganization: SonarCloud organization
- SonarCloudApiKey: SonarCloud token
- SonarCloudHost: SonarCloud host url
- COVERALLS_KEY: **REPO SPECIFIC** access token
- BuildConfiguration: Dotnet Configuration
- NUGET_PACKAGES: if using cache, default is '$(Pipeline.Workspace)/.nuget/packages'

## Visual Studio Code Integration

- Symlink .vscode/tasks-*.json of your choice as .vscode/tasks.json
- Symlink Dotnet.code-workspace to the root of your repo

## Benefits

- Centralized .gitignore for .Net environment
- Common editor settings (for EditorConfig compatible editors)
- Simpler Internals Visible To via csproj properties
- Automatic packing disabled for tests, web projects, services etc
- Automatic addition of stylecop, stylecop configuration and roslyn ruleset on all .net projects under repo
- Embedded Github/Gitlab sourcelink for packable projects under repo
- Resharper configuration that follows stylecop/ruleset configuration
- Fancy xUnit test naming

[buildProps]: Directory.Build.props
[pipeline]: ci/azure-pipelines/azure-pipelines-dotnet.yml
