# syntax=docker/dockerfile:1
# MEGALINTER FLAVOR [security]: Optimized for security
###########################################
###########################################
## Dockerfile to run MegaLinter ##
###########################################
###########################################

# @not-generated

#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################
#ARGTOP__START
# renovate: datasource=docker depName=koalaman/shellcheck
ARG BASH_SHELLCHECK_VERSION=v0.10.0
# renovate: datasource=docker depName=hadolint/hadolint
ARG DOCKERFILE_HADOLINT_VERSION=v2.12.0-alpine
# renovate: datasource=docker depName=ghcr.io/yannh/kubeconform
ARG KUBERNETES_KUBECONFORM_VERSION=v0.6.7-alpine
# renovate: datasource=docker depName=zricethezav/gitleaks
ARG REPOSITORY_GITLEAKS_VERSION=v8.22.1
# renovate: datasource=docker depName=checkmarx/kics
ARG REPOSITORY_KICS_VERSION=v2.1.3-alpine
# renovate: datasource=docker depName=trufflesecurity/trufflehog 
ARG REPOSITORY_TRUFFLEHOG_VERSION=3.88.1
# renovate: datasource=docker depName=ghcr.io/terraform-linters/tflint
ARG TERRAFORM_TFLINT_VERSION=0.54.0
# renovate: datasource=docker depName=tenable/terrascan
ARG TERRAFORM_TERRASCAN_VERSION=1.19.9
# renovate: datasource=docker depName=alpine/terragrunt
ARG TERRAFORM_TERRAGRUNT_VERSION=1.10.3
#ARGTOP__END

#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################
#FROM__START
FROM koalaman/shellcheck:${BASH_SHELLCHECK_VERSION} AS shellcheck
FROM hadolint/hadolint:${DOCKERFILE_HADOLINT_VERSION} AS hadolint
FROM ghcr.io/yannh/kubeconform:${KUBERNETES_KUBECONFORM_VERSION} AS kubeconform
FROM golang:alpine AS dustilock
RUN GOBIN=/usr/bin go install github.com/checkmarx/dustilock@v1.2.0
FROM zricethezav/gitleaks:${REPOSITORY_GITLEAKS_VERSION} AS gitleaks
FROM checkmarx/kics:${REPOSITORY_KICS_VERSION} AS kics
FROM trufflesecurity/trufflehog:${REPOSITORY_TRUFFLEHOG_VERSION} AS trufflehog
FROM ghcr.io/terraform-linters/tflint:v${TERRAFORM_TFLINT_VERSION} AS tflint
FROM tenable/terrascan:${TERRAFORM_TERRASCAN_VERSION} AS terrascan
FROM alpine/terragrunt:${TERRAFORM_TERRAGRUNT_VERSION} AS terragrunt
#FROM__END

##################
# Build wheel for megalinter python package
##################
FROM ghcr.io/astral-sh/uv:0.5.15 AS uv
FROM python:3.12.8-alpine3.20 AS build-ml-core
WORKDIR /
COPY --from=uv /uv /uvx /bin/
# Install dependencies
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project
# Copy the project into the image
COPY . .
# Sync the project
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen

##################
# Get base image #
##################
FROM python:3.12.8-alpine3.20

#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################
#ARG__START

#ARG__END

####################
# Run APK installs #
####################

WORKDIR /

#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################
#APK__START
RUN apk add --no-cache \
                bash \
                ca-certificates \
                curl \
                gcc \
                git \
                git-lfs \
                libffi-dev \
                make \
                musl-dev \
                openssh \
                helm \
                gcompat \
                libc6-compat \
                libstdc++ \
                npm \
                nodejs-current \
                yarn \
    && git config --global core.autocrlf true
#APK__END

# PATH for golang & python
ENV GOROOT=/usr/lib/go \
    GOPATH=/go
    # PYTHONPYCACHEPREFIX="$HOME/.cache/cpython/" NV: not working for all packages :/
# hadolint ignore=DL3044
ENV PATH="$PATH":"$GOROOT"/bin:"$GOPATH"/bin
RUN mkdir -p ${GOPATH}/src ${GOPATH}/bin || true && \
    # Ignore npm package issues
    yarn config set ignore-engines true || true

##############################
# Installs rust dependencies #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

#CARGO__START
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y --profile minimal --default-toolchain stable \
    && export PATH="/root/.cargo/bin:${PATH}" \
    && cargo install --force --locked sarif-fmt  shellcheck-sarif \
    && rm -rf /root/.cargo/registry /root/.cargo/git /root/.cache/sccache /root/.rustup
ENV PATH="/root/.cargo/bin:${PATH}"
#CARGO__END

################################
# Installs python dependencies #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

#PIPVENV__START
RUN PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir --upgrade pip virtualenv \
    && mkdir -p "/venvs/ansible-lint" && cd "/venvs/ansible-lint" && virtualenv . && source bin/activate && PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir ansible-lint && deactivate && cd ./../.. \
    && mkdir -p "/venvs/cfn-lint" && cd "/venvs/cfn-lint" && virtualenv . && source bin/activate && PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir cfn-lint[sarif] && deactivate && cd ./../.. \
    && mkdir -p "/venvs/bandit" && cd "/venvs/bandit" && virtualenv . && source bin/activate && PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir bandit bandit_sarif_formatter bandit[toml] && deactivate && cd ./../.. \
    && mkdir -p "/venvs/checkov" && cd "/venvs/checkov" && virtualenv . && source bin/activate && PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir packaging checkov && deactivate && cd ./../.. \
    && mkdir -p "/venvs/semgrep" && cd "/venvs/semgrep" && virtualenv . && source bin/activate && PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir semgrep && deactivate && cd ./../..  \
    && find /venvs \( -type f \( -iname \*.pyc -o -iname \*.pyo \) -o -type d -iname __pycache__ \) -delete \
    && rm -rf /root/.cache
ENV PATH="${PATH}":/venvs/ansible-lint/bin:/venvs/cfn-lint/bin:/venvs/bandit/bin:/venvs/checkov/bin:/venvs/semgrep/bin
#PIPVENV__END

############################
# Install NPM dependencies #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

ENV NODE_OPTIONS="--max-old-space-size=8192" \
    NODE_ENV=production
#NPM__START
WORKDIR /node-deps
RUN npm --no-cache install --ignore-scripts --omit=dev \
                secretlint \
                @secretlint/secretlint-rule-preset-recommend \
                @secretlint/secretlint-formatter-sarif  && \
    echo "Cleaning npm cache…" \
    && (npm cache clean --force || true) \
    && echo "Changing owner of node_modules files…" \
    && chown -R "$(id -u)":"$(id -g)" node_modules # fix for https://github.com/npm/cli/issues/5900 \
    && echo "Removing extra node_module files…" \
    && find . \( -not -path "/proc" \) -and \( -type f \( -iname "*.d.ts" -o -iname "*.map" -o -iname "*.npmignore" -o -iname "*.travis.yml" -o -iname "CHANGELOG.md" -o -iname "README.md" -o -iname ".package-lock.json" -o -iname "package-lock.json" \) -o -type d -name /root/.npm/_cacache \) -delete 
WORKDIR /

#NPM__END

# Add node packages to path #
ENV PATH="/node-deps/node_modules/.bin:${PATH}" \
    NODE_PATH="/node-deps/node_modules"

##############################
# Installs ruby dependencies #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

#GEM__START

#GEM__END

##############################
# COPY instructions #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

#COPY__START
COPY --link --from=shellcheck /bin/shellcheck /usr/bin/shellcheck
COPY --link --from=hadolint /bin/hadolint /usr/bin/hadolint
COPY --link --from=kubeconform /kubeconform /usr/bin/
COPY --link --from=dustilock /usr/bin/dustilock /usr/bin/dustilock
COPY --link --from=gitleaks /usr/bin/gitleaks /usr/bin/
COPY --link --from=kics /app/bin/kics /usr/bin/kics
COPY --from=kics /app/bin/assets /usr/bin/assets
COPY --link --from=trufflehog /usr/bin/trufflehog /usr/bin/
COPY --link --from=tflint /usr/local/bin/tflint /usr/bin/
COPY --link --from=terrascan /go/bin/terrascan /usr/bin/
COPY --link --from=terragrunt /usr/local/bin/terragrunt /usr/bin/
#COPY__END

#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################
#OTHER__START
# bash-exec installation
RUN printf '#!/bin/bash \n\nif [[ -x "$1" ]]; then exit 0; else echo "Error: File:[$1] is not executable"; exit 1; fi' > /usr/bin/bash-exec \
    && chmod +x /usr/bin/bash-exec \
#
# shellcheck installation
# Managed with COPY --link --from=shellcheck /bin/shellcheck /usr/bin/shellcheck
#
# hadolint installation
# Managed with COPY --link --from=hadolint /bin/hadolint /usr/bin/hadolint
#
# kubeconform installation
# Managed with COPY --link --from=kubeconform /kubeconform /usr/bin/
#
# kubescape installation
    && ln -s /lib/libc.so.6 /usr/lib/libresolv.so.2 && \
    curl --retry 5 --retry-delay 5 -sLv https://raw.githubusercontent.com/kubescape/kubescape/master/install.sh | /bin/bash -s -- -v v2.9.0 \
#
# devskim installation
    && apk add --no-cache dotnet8-sdk
ENV PATH="${PATH}:/root/.dotnet/tools"
RUN dotnet tool install --global Microsoft.CST.DevSkim.CLI \
#
# dustilock installation
# Managed with COPY --link --from=dustilock /usr/bin/dustilock /usr/bin/dustilock
#
# gitleaks installation
# Managed with COPY --link --from=gitleaks /usr/bin/gitleaks /usr/bin/
#
# grype installation
    && curl -sSfL https://raw.githubusercontent.com/anchore/grype/main/install.sh | sh -s -- -b /usr/local/bin v0.79.5
#
# kics installation
# Managed with COPY --link --from=kics /app/bin/kics /usr/bin/kics
ENV KICS_QUERIES_PATH=/usr/bin/assets/queries KICS_LIBRARIES_PATH=/usr/bin/assets/libraries
# Managed with COPY --from=kics /app/bin/assets /usr/bin/assets
#
# syft installation
RUN curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b /usr/local/bin \
#
# trivy installation
    && wget --tries=5 -q -O - https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin \
    && (trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress)

#
# trivy-sbom installation
# Next line commented because already managed by another linter
# RUN wget --tries=5 -q -O - https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin \
#     && (trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress || trivy image --download-db-only --no-progress)
#
# trufflehog installation
# Managed with COPY --link --from=trufflehog /usr/bin/trufflehog /usr/bin/
#
# tflint installation
# Managed with COPY --link --from=tflint /usr/local/bin/tflint /usr/bin/
#
# terrascan installation
# Managed with COPY --link --from=terrascan /go/bin/terrascan /usr/bin/
#
# terragrunt installation
# Managed with COPY --link --from=terragrunt /usr/local/bin/terragrunt /usr/bin/
#
#OTHER__END

################################
# Installs python dependencies #
################################
COPY --from=build-ml-core pyproject.toml README.md ./
COPY --from=build-ml-core megalinter /megalinter/
RUN --mount=type=cache,target=/root/.cache/uv,from=build-ml-core \
    --mount=from=uv,source=/uv,target=/bin/uv \
    uv pip install --system -e .

#######################################
# Copy scripts and rules to container #
#######################################
COPY megalinter/descriptors /megalinter-descriptors
COPY TEMPLATES /action/lib/.automation

# Copy server scripts
COPY server /server

###########################
# Get the build arguments #
###########################
ARG BUILD_DATE
ARG BUILD_REVISION
ARG BUILD_VERSION

#################################################
# Set ENV values used for debugging the version #
#################################################
ENV BUILD_DATE=$BUILD_DATE \
    BUILD_REVISION=$BUILD_REVISION \
    BUILD_VERSION=$BUILD_VERSION

#FLAVOR__START
ENV MEGALINTER_FLAVOR=security
#FLAVOR__END

#########################################
# Label the instance and set maintainer #
#########################################
LABEL com.github.actions.name="MegaLinter" \
      com.github.actions.description="The ultimate linters aggregator to make sure your projects are clean" \
      com.github.actions.icon="code" \
      com.github.actions.color="red" \
      maintainer="Nicolas Vuillamy <nicolas.vuillamy@gmail.com>" \
      org.opencontainers.image.created=$BUILD_DATE \
      org.opencontainers.image.revision=$BUILD_REVISION \
      org.opencontainers.image.version=$BUILD_VERSION \
      org.opencontainers.image.authors="Nicolas Vuillamy <nicolas.vuillamy@gmail.com>" \
      org.opencontainers.image.url="https://megalinter.io" \
      org.opencontainers.image.source="https://github.com/oxsecurity/megalinter" \
      org.opencontainers.image.documentation="https://megalinter.io" \
      org.opencontainers.image.vendor="Nicolas Vuillamy" \
      org.opencontainers.image.description="Lint your code base with GitHub Actions"

#EXTRA_DOCKERFILE_LINES__START
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x entrypoint.sh
ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]
#EXTRA_DOCKERFILE_LINES__END
