# Copyright (C) 2026 Sam Dornan
# This program is free software: you can redistribute it and/or modify it under the terms of the GNU Affero General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

# ==========================================
# STAGE 1: The Harvester (Pure Trixie Edition)
# ==========================================

# 0. Uses standard Debian for testing and harvesting the necessary binaries and libraries
FROM debian:13@sha256:3cda3ca472ce45507a5109c6a3907093e24d9ae67d55954c9a07a45665dafba2 AS harvester

# 1. Install prerequisites
RUN apt-get update && apt-get install -y curl ca-certificates

# 2. Securely prepare the keyring directory
RUN mkdir -p /usr/share/keyrings

# 3. Install required underlying utilities
RUN apt-get update && apt-get install -y curl

# ==========================================
# STAGE 2: The Final Base (Distroless Debian 13)
# ==========================================

#0. Uses debug version of Distroless base image, which includes a busybox shell to run bash scripts.
FROM gcr.io/distroless/base-debian13:debug@sha256:48858464b973bfab94db426b4e4626114a296c21e127404b6acc3367a3bc172e

# 1. Bring over the binaries
COPY --from=harvester /usr/bin/curl /usr/bin/curl

# 2. Bring over the x86_64 shared libraries required by those binaries
COPY --from=harvester /lib/x86_64-linux-gnu/ /lib/x86_64-linux-gnu/
COPY --from=harvester /usr/lib/x86_64-linux-gnu/ /usr/lib/x86_64-linux-gnu/

# 3. Bring over the x86_64 dynamic linker (Note the path change to /lib64)
COPY --from=harvester /lib64/ld-linux-x86-64.so.2 /lib64/ld-linux-x86-64.so.2

ENV PATH="/usr/bin:/bin"
ENV LD_LIBRARY_PATH="/lib/x86_64-linux-gnu:/usr/lib/x86_64-linux-gnu"