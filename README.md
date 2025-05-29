# Reusable Workflows for .NET CLI Build

This repository contains reusable workflows for interacting with .NET CLI `build` command in your CI/CD pipeline.

> These workflows is part of the Codebelt umbrella and ensures a consistent way of: 
> 
> - Defining your CI/CD pipeline 
> - Structuring your repository
> - Keeping your codebase small and feasible
> - Writing clean and maintainable code
> - Deploying your code to different environments
> - Automating as much as possible
>
> A paved path to excel as a DevSecOps Engineer.

## Available Workflows

- [default.yml](.github/workflows/default.yml) - the `dotnet build` workflow that:
  - [fetches the codebase](https://github.com/codebeltnet/git-checkout),
  - [installs the .NET SDK](https://github.com/codebeltnet/install-dotnet),
  - [installs the MinVer for .NET tool](https://github.com/codebeltnet/dotnet-tool-install-minver),
  - [conditionally downloads strong name signing key](https://github.com/codebeltnet/gcp-download-file),
  - [conditionally restores the dependencies](https://github.com/codebeltnet/dotnet-restore),
  - conditionally restores cached content,
  - [builds the solution](https://github.com/codebeltnet/dotnet-build),
  - uploads the built workspace as a workflow artifact.

### Usage

To call this workflow in your GitHub repository, you can follow these steps:

```yaml
build-call:
    uses: codebeltnet/jobs-dotnet-build/.github/workflows/default.yml@v2
```

#### Inputs

```yaml
with:
  # Path to the project(s) file to build. Pass empty to have MSBuild use the default behavior. Supports globbing.  Default is an empty string.
  projects: ''
  # Defines the build configuration. Default is Debug.
  configuration: 'Debug'
  # Compiles for a specific framework. The framework must be defined in the project file. Default is an empty string.
  framework: ''
  # When set to true, includes preview versions of .NET. Default is false.
  include-preview: false
  # The filename of the strong name key. Default is an empty string.
  strong-name-key-filename: ''
  # Sets the verbosity level of the command. Allowed values are quiet, minimal, normal, detailed, and diagnostic. Default is quiet.
  verbosity-level: 'quiet'
  # Provides a way to fully customize the build. Default is empty. See https://learn.microsoft.com/en-us/visualstudio/msbuild/msbuild-command-line-reference?view=vs-2022#switches for more information.
  build-switches: ''
  # Upload the generated build artifact. Default is to upload.
  upload-build-artifact: true
  # The name of the uploaded build artifact. Default, when left empty, is 'format('{0}-{1}', inputs.framework, inputs.configuration)'.
  upload-build-artifact-name: ''
  # When set, the current workspace will be overwritten with the content of the restore cache. Default is an empty string.
  restore-cache-key: ''
  # The maximum time in minutes to allow the job to run. Default is 15 minutes.
  timeout-minutes: 15
```

#### Secrets

```yaml
secrets:
  GCP_TOKEN: ${{ secrets.GCP_TOKEN }}
  GCP_BUCKETNAME: ${{ secrets.GCP_BUCKETNAME }}
```

#### Outputs

```yaml
outputs:
  # The calculated version in SEMVER format.
  version: X.Y.Z # where X is MAJOR, Y is MINOR and Z is PATCH
```

#### Examples

```yaml
# Sign the assembly with a strong name key
jobs:
  build:
    uses: codebeltnet/jobs-dotnet-build/.github/workflows/default@v2
    with:
      strong-name-key-filename: 'my-key.snk'
    secrets: inherit

# Time-consuming builds for large projects
- name: Build
  uses: codebeltnet/dotnet-build@v4
  with:
    restore-cache-key: dotnet-restore-sha256
```

## Caller workflows to showcase the Codebelt experience

### Basic CI/CD Pipeline

- Bootstrapper API - https://github.com/codebeltnet/bootstrapper/blob/main/.github/workflows/pipelines.yml
- Extensions for Asp.Versioning API - https://github.com/codebeltnet/asp-versioning/blob/main/.github/workflows/pipelines.yml
- Extensions for AWS Signature Version 4 API - https://github.com/codebeltnet/aws-signature-v4/blob/main/.github/workflows/pipelines.yml
- Extensions for Globalization API - https://github.com/codebeltnet/globalization/blob/main/.github/workflows/pipelines.yml
- Extensions for Newtonsoft.Json API - https://github.com/codebeltnet/newtonsoft-json/blob/main/.github/workflows/pipelines.yml
- Extensions for Swashbuckle.AspNetCore API - https://github.com/codebeltnet/swashbuckle-aspnetcore/blob/main/.github/workflows/pipelines.yml
- Extensions for xUnit API - https://github.com/codebeltnet/xunit/blob/main/.github/workflows/pipelines.yml
- Extensions for YamlDotNet API - https://github.com/codebeltnet/yamldotnet/blob/main/.github/workflows/pipelines.yml
- Shared Kernel API - https://github.com/codebeltnet/shared-kernel/blob/main/.github/workflows/pipelines.yml
- Unitify API - https://github.com/codebeltnet/unitify/blob/main/.github/workflows/pipelines.yml

### Intermediate CI/CD Pipeline

- Savvy I/O - https://github.com/codebeltnet/savvyio/blob/main/.github/workflows/pipelines.yml

### Advanced CI/CD Pipeline

- Cuemon for .NET - https://github.com/gimlichael/Cuemon/blob/main/.github/workflows/pipelines.yml

## Contributing to Reusable Workflows for .NET CLI Build

Contributions are welcome! 
Feel free to submit issues, feature requests, or pull requests to help improve these workflows.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
