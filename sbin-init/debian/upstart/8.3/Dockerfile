FROM debian:8.3

# stoled from https://github.com/tianon/dockerfiles

# we're going to want this bad boy installed so we can connect :)
RUN apt-get update && apt-get install -y ssh locales upstart systemd-shim systemd-sysv- --yes --no-install-recommends

ADD init-fake.conf /etc/init/fake-container-events.conf

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen

# undo some leet hax of the base image
RUN rm /usr/sbin/policy-rc.d; \
	rm /sbin/initctl; dpkg-divert --rename --remove /sbin/initctl

# generate a nice UTF-8 locale for our use
RUN locale-gen en_US.UTF-8 && update-locale LANG=en_US.UTF-8

# remove some pointless services
RUN /usr/sbin/update-rc.d -f ondemand remove; \
	for f in \
		/etc/init/u*.conf \
		/etc/init/mounted-dev.conf \
		/etc/init/mounted-proc.conf \
		/etc/init/mounted-run.conf \
		/etc/init/mounted-tmp.conf \
		/etc/init/mounted-var.conf \
		/etc/init/hostname.conf \
		/etc/init/networking.conf \
		/etc/init/tty*.conf \
		/etc/init/plymouth*.conf \
		/etc/init/hwclock*.conf \
		/etc/init/module*.conf\
	; do \
		dpkg-divert --local --rename --add "$f"; \
	done; \
	echo '# /lib/init/fstab: cleared out for bare-bones Docker' > /lib/init/fstab

# small fix for SSH in 13.10 (that's harmless everywhere else)
RUN sed -ri 's/^session\s+required\s+pam_loginuid.so$/session optional pam_loginuid.so/' /etc/pam.d/sshd

RUN sed -ri 's/^PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config

# let Upstart know it's in a container
ENV container docker

# set a cheap, simple password for great convenience
RUN echo 'root:docker.io' | chpasswd

# we can has SSH
EXPOSE 22

# prepare for takeoff
CMD ["/sbin/init"]
