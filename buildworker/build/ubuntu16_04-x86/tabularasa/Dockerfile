## This file was autogenerated
# Do not edit directly; edit the template files
FROM staticfloat/julia_tabularasa:ubuntu16_04-x86
USER root
RUN mkdir -p /buildworker
RUN chown buildworker:buildworker /buildworker
WORKDIR /buildworker

# These arguments are passed in from `docker-compose`
ARG L32
ARG buildbot_server
ARG buildbot_port
ARG buildbot_password
ARG buildworker_name

# First, install the buildbot-worker package (no need for virtualenv because
# we're within a Docker container!), and cleanup pip cache once we're done
RUN $L32 pip install buildbot-worker
RUN rm -rf /root/.cache

# Install our startup script
COPY start_worker.sh /buildworker/start_worker.sh
RUN chmod +x /buildworker/start_worker.sh
CMD ["/buildworker/start_worker.sh"]

# Switch back to our buildworker user, and configure the buildbot-worker!
USER buildworker
RUN $L32 buildbot-worker create-worker --keepalive=100 --umask 022 worker $buildbot_server:$buildbot_port $buildworker_name $buildbot_password
RUN echo "Elliot Saba <staticfloat@gmail.com>" > worker/info/admin
RUN echo "Julia $buildworker_name buildworker" > worker/info/host

