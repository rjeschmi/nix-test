FROM centos:7

ADD ./nix-1.10-x86_64-linux.tar.bz2 /tmp

WORKDIR /tmp/nix-1.10-x86_64-linux

RUN yum install -y sudo
RUN groupadd -r nixbld
RUN for n in $(seq 1 10); do useradd -c "Nix build user $n" \
    -d /var/empty -g nixbld -G nixbld -M -N -r -s "$(which nologin)" \
    nixbld$n; done

RUN mkdir -m 0755 /nix && chown root /nix
RUN bash -l -c ./install
RUN ln -s /nix/var/nix/profiles/default/etc/profile.d/nix.sh /etc/profile.d

RUN groupadd nix-users
RUN useradd -m build -g nix-users

#RUN chgrp nix-users /nix/var/nix/daemon-socket
#RUN chmod ug=rwx,o= /nix/var/nix/daemon-socket

RUN bash -l -c 'nix-channel --update'
RUN bash -l -c 'nix-env -i git'

RUN mkdir -p /root/git

WORKDIR /root/git

RUN bash -l -c 'git clone https://github.com/nixos/nixpkgs'

RUN mkdir /etc/nix

RUN bash -l -c 'nix-env -i man'
ADD nix.conf /etc/nix

VOLUME /export

