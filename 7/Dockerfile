FROM centos:centos7
LABEL maintainer="Yuri Corona Lopes"

RUN yum update -y && \
    yum upgrade -y && \
    yum install -y systemd systemd-cron python3 python3-pip sudo bash ca-certificates iproute glibc-langpack-en && \
    rm -Rf /usr/share/doc && \
    rm -Rf /usr/share/man && \
    yum clean all

RUN rm -f /lib/systemd/system/multi-user.target.wants/* \
    /etc/systemd/system/*.wants/* \
    /lib/systemd/system/local-fs.target.wants/* \
    /lib/systemd/system/sockets.target.wants/*udev* \
    /lib/systemd/system/sockets.target.wants/*initctl* \
    /lib/systemd/system/sysinit.target.wants/systemd-tmpfiles-setup* \
    /lib/systemd/system/systemd-update-utmp*

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# Create `ansible`
ENV ANSIBLE_USER=ansible
RUN set -xe && \
    groupadd -r ${ANSIBLE_USER} && \ 
    useradd -m -g ${ANSIBLE_USER} ${ANSIBLE_USER}

RUN set -xe && \
    echo "${ANSIBLE_USER} ALL=(ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/ansible

# Custom bashrc
RUN printf "# .bashrc \n\n\
alias pip=pip3 \n\
alias python=python3 \n\
alias ll='ls -l --color=auto' \n\
alias rm='rm -i' \n\
alias cp='cp -i' \n\
alias mv='mv -i' \n\n\
if [ -f /etc/bashrc ]; then \n\
	. /etc/bashrc \n\
fi \n\
" > /root/.bashrc

VOLUME [ "/sys/fs/cgroup" ]

CMD [ "/sbin/init" ]