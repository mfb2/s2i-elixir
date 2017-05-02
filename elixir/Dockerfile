FROM elixir

MAINTAINER Daniel Dreier <d@puppet.com>

EXPOSE 5000

ENV \
  PORT=5000 \
  MIX_ENV=prod \
  NPM_VERSION=3 \
  NODE_ENV=production \
  DEL_PKGS="libgcc libstdc++" \
  RM_DIRS=/usr/include \
  STI_SCRIPTS_PATH=/usr/libexec/s2i \
  REFRESHED_AT=2016-06-07

LABEL \
  io.k8s.description="Platform for building and running Elixir applications" \
  io.k8s.display-name="Elixir v1.4.2, Node.js v6.2" \
  io.openshift.expose-services="5000:http" \
  io.openshift.tags="builder,elixir,elixir-1.4.2" \
  io.s2i.scripts-url=image:///usr/libexec/s2i \
  com.redhat.deployments-dir="$HOME"

RUN mkdir -p /opt/app-root
RUN mix local.hex --force && \
    mix local.rebar --force

# Copy the S2I scripts
COPY ./bin/ $STI_SCRIPTS_PATH

# Drop the root user and make the content of $HOME owned by user 1001
RUN chown -R 1001:0 /opt/app-root $STI_SCRIPTS_PATH
USER 1001

# Set the default CMD to print the usage for this iamge
CMD $STI_SCRIPTS_PATH/usage
