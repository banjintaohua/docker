FROM gitlab/gitlab-runner

LABEL maintainer="banjintaohua"

ENV USER gitlab-runner

RUN echo "StrictHostKeyChecking no" >> /etc/ssh/ssh_config \
    && echo "UserKnownHostsFile /dev/null" >> /etc/ssh/ssh_config

WORKDIR /home/gitlab-runner
