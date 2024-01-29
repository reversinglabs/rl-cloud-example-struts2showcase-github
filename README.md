# ReversingLabs rl-secure-cloud GitHub Actions Examples

This repository contains working examples of GitHub workflows to illustrate scanning with the
[ReversingLabs secure.software Portal](https://docs.secure.software/portal/integrations/).

ReversingLabs secure.software Portal is capable of scanning
[nearly any type](https://docs.secure.software/concepts/language-coverage)
of software artifact or package that results from a build.

In this example, we use the source code and Maven build instructions for the Struts2 showcase web app,
which came with [Apache Struts v2.5.28](https://archive.apache.org/dist/struts/2.5.28/).

The following examples are provided in the `.github/workflows` directory:

- **rl-scan-docker.yml**
- **rl-scan-docker-only.yml**

The all examples require that you define the `RLPORTAL_ACCESS_TOKEN` secret environment variable to store your ReversingLabs
[Portal access token](https://docs.secure.software/api/generate-api-token).
You can define the secrets either in your GitHub repository or in the GitHub organization settings.


## rl-scan-docker.yml

This workflow builds the WAR file and scans it with the Portal using the [ReversingLabs rl-scanner-cloud Docker image](https://hub.docker.com/r/reversinglabs/rl-scanner-cloud).

The workflow is configured to be triggered manually only.

After the file is scanned, analysis reports are published as an artifact called "ReversingLabs reports".


## rl-scan-docker-only.yml

This workflow shows how to use the **[reversinglabs/gh-action-rl-scanner-cloud-only](https://github.com/marketplace/actions/gh-action-rl-scanner-cloud-only)** GitHub Action that relies on the `reversingLabs/rl-scanner-cloud` Docker image to scan the build artifact.


**Supported parameters**

| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `RLPORTAL_ACCESS_TOKEN` | *Yes* | string | A Personal Access Token for authenticating requests to the secure.software Portal. Before you can use this example, you must [create the token](https://docs.secure.software/api/generate-api-token) in your Portal settings. Tokens can expire and be revoked, in which case you'll have to update this value. Define it as a secret in a group `rl-scanner-cloud` |
| `RLPORTAL_SERVER`      | *Yes* | string | Name of the secure.software Portal instance to use for the scan. The Portal instance name usually matches the subdirectory of `my.secure.software` in your Portal URL. For example, if your portal URL is `my.secure.software/demo`, the instance name to use with this parameter is `demo`. |
| `RLPORTAL_ORG`         | *Yes* | string | The name of a secure.software Portal organization to use for the scan. The organization must exist on the Portal instance specified with `RLPORTAL_SERVER`. The user account authenticated with the token must be a member of the specified organization and have the appropriate permissions to upload and scan a file. Organization names are case-sensitive. |
| `RLPORTAL_GROUP`       | *Yes* | string | The name of a secure.software Portal group to use for the scan. The group must exist in the Portal organization specified with `RLPORTAL_ORG`. Group names are case-sensitive. |
| `REPORT_PATH`           | No | string | Path to the location where you want to store analysis reports. Must be relative to the `System.DefaultWorkingDirectory` |


## Pipeline result

The scan in this example will produce the `FAIL` CI status.
The pipeline will fail, as the scanner detects 4 critical issues: `Found 4 vulnerabilities matching selected criteria.`

## Useful resources

  1. [The official secure.software Portal documentation](https://docs.secure.software/portal/)
  2. The official `reversinglabs/rl-scanner-cloud` Docker image [on Docker Hub](https://hub.docker.com/r/reversinglabs/rl-scanner-cloud)

