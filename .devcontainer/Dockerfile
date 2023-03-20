# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.208.0/containers/dotnet/.devcontainer/base.Dockerfile

# [Choice] .NET version: 6.0, 5.0, 3.1, 6.0-bullseye, 5.0-bullseye, 3.1-bullseye, 6.0-focal, 5.0-focal, 3.1-focal
ARG VARIANT="7.0-bullseye-slim"
FROM mcr.microsoft.com/vscode/devcontainers/dotnet:0-${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# Install Mono
RUN apt-get -qqy update && \
    apt-get -qqy install apt-transport-https dirmngr gnupg ca-certificates && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
    echo "deb https://download.mono-project.com/repo/debian stable-buster main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list && \
    apt-get -qqy update && \
    apt-get -qqy install mono-devel msbuild && \
    rm -rf /var/lib/apt/lists/*

# Install Docfx
RUN mkdir /usr/local/share/docfx && \
    cd /usr/local/share/docfx && \
    curl -sSL -o docfx.zip https://github.com/dotnet/docfx/releases/download/v2.59.2/docfx.zip && \
    unzip docfx.zip && \
    rm docfx.zip

COPY docfx /usr/local/bin/docfx
