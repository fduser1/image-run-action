
# Image Run GitHub Action

This GitHub Action allows you to run a shell script against a docker image.

>NOTE: GitHub Actions does not support the `run` script property in combination with a Docker container image (e.g. `uses: docker://ubuntu:latest`). This action workarounds this limitation.

## Usage

### Inputs

| Name                  | Requirement | Description |
| --------------------- | ----------- | ----------- |
| `image`    | __required__ | Docker image location. Refer to [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) documentation for location format; e.g.<br/><small>  ubuntu:22.04, ubuntu@sha256:c9cf959...., google/cloud-sdk@latest, public.ecr.aws/docker/library/python:3.9</small> |
| `run`      | __required__ | Shell script to run. _(Do not include shebang)_ |
| `shell`    |  _optional_  | Default: `sh`. Shell within image to run the script; e.g. <small> bash, dash, /usr/bin/zsh</small>  |
| `options`  |  _optional_  | Additional options to set as a part of the [docker run](https://docs.docker.com/engine/reference/commandline/run/) command. |


### Examples

```yaml
- uses: fduser1/image-run-action@v1
  with:
    image: python:3.9.17-slim
    run: |
      echo "hello world"
      python --version
```


```yaml
- uses: fduser1/image-run-action@v1
  with:
    image: public.ecr.aws/docker/library/python:3.9.17-slim
    shell: bash
    run: |
      echo "hello world"
      python --version
```

## Docker Run

This action mimics GitHub Actions' context configuration (e.g. environment variables, volume mounts) for docker runs.

See [DockerCommandManager.cs](https://github.com/actions/runner/blob/main/src/Runner.Worker/Container/DockerCommandManager.cs) and [ContainerActionHandler.cs](https://github.com/actions/runner/blob/main/src/Runner.Worker/Handlers/ContainerActionHandler.cs) for GitHub Actions's docker run behavior and settings.

The following step was used as the baseline for our docker runs:
<small>
\- uses: docker://ubuntu:22.04<br/>
&nbsp;&nbsp;with:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;entrypoint: bash<br/>
&nbsp;&nbsp;&nbsp;&nbsp;args: -c env<br/>
</small>

