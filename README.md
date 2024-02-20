# Buildkite Rails Docker Test Pipeline

This README outlines the steps to set up and use a Buildkite pipeline for testing a Rails application within a Docker environment. This setup is intended for the `loftwah` organization's test pipeline on Buildkite, running on the latest Ubuntu on AWS and WSL2 locally.

## Prerequisites

Before proceeding, ensure you have the following:

- An account on Buildkite linked to the `loftwah` organization.
- A starter pipeline created on Buildkite.
- Docker and Docker Compose installed on your system.
- The latest version of Ubuntu running on AWS or WSL2 for local development.

## Setting Up Buildkite Agent

First, install and configure the Buildkite agent on your Ubuntu system. This agent will handle job execution for your pipeline.

### 1. Add Buildkite's APT repository:

```bash
sudo sh -c 'echo "deb https://apt.buildkite.com/buildkite-agent stable main" > /etc/apt/sources.list.d/buildkite-agent.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 32A37959C2FA5C3C99EFBC32A79206696452D198
```

### 2. Install the Buildkite agent:

```bash
sudo apt-get update && sudo apt-get install -y buildkite-agent
```

### 3. Configure your agent token:

Replace `your-agent-token-here` with your actual Buildkite agent token.

```bash
sudo sed -i "s/xxx/your-agent-token-here/g" /etc/buildkite-agent/buildkite-agent.cfg
```

### 4. Start the Buildkite agent:

For Ubuntu 20.04 and above:

```bash
sudo systemctl enable buildkite-agent && sudo systemctl start buildkite-agent
```

## Running Your Pipeline

Pipeline needs to be set up on Buildkite before running it. The pipeline configuration file is located at `.buildkite/pipeline.yml`. This file contains the steps and commands to be executed by the Buildkite agent.

Commit and push your changes to your repository to trigger a build in Buildkite. Monitor the build's progress and output directly from the Buildkite dashboard.

## Viewing Logs

To view the Buildkite agent's logs on your system:

```bash
journalctl -f -u buildkite-agent
```

## Running the Pipeline

To run the pipeline locally, use the following command:

```bash
curl -H "Authorization: Bearer bkua_thisisademokey" "https://api.buildkite.com/v2/organizations/loftwah/pipelines/rails-demo/builds" \
  -X "POST" \
  -F "commit=HEAD" \
  -F "branch=main" \
  -F "message=First build :rocket:"
  ```

## Conclusion

This setup provides a basic continuous integration pipeline for a Rails application using Docker and Buildkite. Adjust the Docker and pipeline configurations according to your project's requirements.

For more detailed information on Buildkite plugins, Docker setup, or Rails configurations, refer to their respective documentation.
