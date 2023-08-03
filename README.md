# vul-plugin-khulnasoft

Vul plugin for integration with KhulnaSoft Security SaaS platform

## Usage

Vul's options need to be passed after `--`. Vul receives a target directory containing IaC files

Set KhulnaSoft plugin as Vul's current default plugin by exporting an environment variable

```
  export VUL_RUN_AS_PLUGIN=khulnasoft
```

### Scan an IaC target

```
  vul  <target>
```

### Scan an IaC target and tag the scan

```
  vul  <target>
```

### Scan an IaC target and report only specific severities

```
  vul --severities CRITICAL,HIGH <target>
```

## Command Line Arguments

| Argument         | Purpose                                    | Example Usage                                 |
| ---------------- | ------------------------------------------ | --------------------------------------------- |
| `--debug`        | Get more detailed output as Vul runs.    |                                               |
| `--severities`   | The Severities that you are interested in. | `--severities CRITICAL,HIGH,UNKNOWN`          |
| `--tags`         | Arbitrary tags to be stored with the scan. | `--tags 'BUILD_HOST=$HOSTNAME,foo=bar'`       |
| `--pipelines`    | Scan repository pipeline files.            | `--pipelines` / `PIPELINES=1 vul ...`       |
| `--package-json` | Scan package.json files without lock files | `--package-json` / `PACKAGE_JSON=1 vul ...` |

## Environment Variables

### Required

The only explicitly required environment variables are

| Variable    | Purpose                                                       |
|:------------|:--------------------------------------------------------------|
| KHULNASOFT_KEY    | Generated through CSPM UI                                     |
| KHULNASOFT_SECRET | Generated through CSPM UI                                     |


### Optional

| Variable    | Purpose                                                       |
|:------------|:--------------------------------------------------------------|
| CSPM_URL    | URL to generate KhulnaSoft Platform token (default: us-east-1 CSPM) |
| KHULNASOFT_URL    | KhulnaSoft platform URL (default: us-east-1 KhulnaSoft platform)          |



Vul will attempt to resolve the following details from the available environment variables;

- repository name
- branch name
- commit id
- committing user
- build system

There are some special case env vars;

| Variable             | Purpose                                                                                |
| :------------------- | :------------------------------------------------------------------------------------- |
| OVERRIDE_REPOSITORY  | Use this environment variable to explicitly specify the repository used by Vul       |
| FALLBACK_REPOSITORY  | Use this environment variable as a backup if no other repository env vars can be found |
| OVERRIDE_BRANCH      | Use this environment variable to explicitly specify the branch used by Vul           |
| FALLBACK_BRANCH      | Use this environment variable as a backup if no other branch env vars can be found     |
| OVERRIDE_BUILDSYSTEM | Use this environment variable to explicitly specify the build system                   |
| OVERRIDE_SCMID       | Use this environment variable to explicitly specify the scm id                         |
| IGNORE_PANIC         | Use this environment variable to return exit code 0 on cli panic                       |
| OVERRIDE_REPOSITORY_URL  | Use this environment variable to explicitly specify the repository link used by Vul (For result's web link)       |
| OVERRIDE_REPOSITORY_SOURCE  | Use this environment variable to explicitly specify the repository source used by Vul       |

# Scanners

Certain scanners have additional behaviors

### Pipelines

The pipelines scanner is enabled by providing either `--pipelines` flag or `PIPELINES=1` environment variable.
It uses [Pipeline Parser](https://github.com/khulnasoft-labs/pipeline-parser) to parse the pipelines, and therefore, supports only the platforms that are supported by the package.

The results of the scanner are:

- parsed version of the pipeline files
- pipeline misconfigurations
