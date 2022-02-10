# Jenkins GitHub Action

Start Jenkins jobs with GitHub actions. Reports back the Jenkins URL and status of the build.

## Usage

You need to setup a Jenkins API token if you require authentication with Jenkins:

1. Log in Jenkins.
2. Click you name (upper-right corner).
3. Click Configure (left-side menu).
4. Use "Add new Token" button to generate a new one then name it.
5. You must copy the token when you generate it as you cannot view the token afterwards.

It's best practice to save the token in [GitHub secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets).

### Example workflow

```yaml
name: jenkins-CI

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: Trigger jenkins job
        uses: joshlk/jenkins-githubaction@master
        with:
          url: https://jenkins_url
          job_name: jenkins_job_name
          username: ${{ secrets.JENKINS_USER }}
          api_token: ${{ secrets.JENKINS_TOKEN }}
          timeout: "1000"
```

### Inputs

| Input                                   | Description    | Default |
| ------------- | ------------- | ------------- |
| `url`  | Jenkins URL including http/https protocol  |  |
| `job_name` | Jenkins job name to build   | |
| `username` _(optional)_  | Jenkins username   | |
| `api_token` _(optional)_  | Jenkins API token   | |
| `parameters` _(optional)_  | Build parameters in JSON format e.g. `{"field1":"value1"}`   | |
| `cookies` _(optional)_  | Cookies to include in HTTP requests in JSON format e.g. `{"field1":"value1"}`   | |
| `wait` _(optional)_  | Should the runner wait for the build to finish and provide ok status   | True |
| `timeout` _(optional)_  | Timeout in seconds for build to complete   | 600 |
| `start_timeout` _(optional)_  | Timeout in seconds for build to start | 600 |
| `interval` _(optional)_  | ow frequently in seconds to query Jenkins for build status  | 5 |

### Outputs

| Output                                             | Description                                        |
|------------------------------------------------------|-----------------------------------------------|
| `build_url`  | Jenkins build URL  |

