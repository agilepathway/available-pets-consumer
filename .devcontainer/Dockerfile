# [Choice] Ruby version (use -bullseye variants on local arm64/Apple Silicon): 3, 3.0, 2, 2.7, 2.6, 3-bullseye, 3.0-bullseye, 2-bullseye, 2.7-bullseye, 2.6-bullseye, 3-buster, 3.0-buster, 2-buster, 2.7-buster, 2.6-buster
ARG VARIANT=2-bullseye
FROM mcr.microsoft.com/vscode/devcontainers/ruby:${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] Uncomment this section to install additional OS packages.
ARG CHROMIUM_VERSION="none"
 RUN if [ "${CHROMIUM_VERSION}" != "none" ]; then apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends chromium="${CHROMIUM_VERSION}" \
# Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*; fi

# [Optional] Uncomment this line to install additional gems.
RUN gem install bundler rubocop

# [Optional] Uncomment this line to install global node packages.
RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g @stoplight/prism-cli" 2>&1

# Install gauge and gauge plugins
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN curl -SsL https://downloads.gauge.org/stable | sh \
    && su vscode -c "gauge install html-report" \
    && su vscode -c "gauge install screenshot" \
    && su vscode -c "gauge install js"
