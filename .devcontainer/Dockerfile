FROM steven0351/ghc-stack-hie:8.6.5

# This Dockerfile adds a non-root user with sudo access. Use the "remoteUser"
# property in devcontainer.json to use it. On Linux, the container user's GID/UIDs
# will be updated to match your local UID/GID (when using the dockerFile property).
# See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Set to false to skip installing zsh and Oh My ZSH!
ARG INSTALL_ZSH="false"

# Location and expected SHA for common setup script - SHA generated on release
ARG COMMON_SCRIPT_SOURCE="https://raw.githubusercontent.com/microsoft/vscode-dev-containers/master/script-library/common-debian.sh"
ARG COMMON_SCRIPT_SHA="dev-mode" 

RUN apt-get update \
    && apt-get -y install --no-install-recommends ca-certificates libicu-dev libgmp-dev 2>&1 \
    && curl -sSL  ${COMMON_SCRIPT_SOURCE} -o /tmp/common-setup.sh \
    && ([ "${COMMON_SCRIPT_SHA}" = "dev-mode" ] || (echo "${COMMON_SCRIPT_SHA} */tmp/common-setup.sh" | sha256sum -c -)) \
    && /bin/bash /tmp/common-setup.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" \
    && rm /tmp/common-setup.sh

RUN cp -r /root/.local /home/vscode/ && \
    chmod 755 -R /opt/cabal/2.4/bin && \
    chmod 755 -R /opt/ghc/8.6.5/bin 

ENV PATH /root/.cabal/bin:/home/vscode/.local/bin:/opt/cabal/2.4/bin:/opt/ghc/8.6.5/bin:$PATH