FROM nginx


RUN apt-get update && \
    apt-get install -y gcc && \
    apt-get install -y nginx && \
    apt-get install -y libfcgi-dev && \
    apt-get install -y spawn-fcgi && \
    rm -rf /var/lib/apt/lists && \
    useradd -d /home/cityorde -m -s /bin/bash cityorde

RUN chmod g-s sbin/unix_chkpwd && \
	chmod u-s usr/bin/chsh && \
	chmod g-s usr/bin/wall && \
	chmod u-s bin/umount && \
	chmod u-s usr/bin/passwd && \
	chmod u-s usr/bin/chfn && \
	chmod u-s usr/bin/gpasswd && \
	chmod g-s usr/bin/expiry && \
	chmod g-s usr/bin/chage && \
	chmod u-s usr/bin/newgrp && \
	chmod u-s bin/mount && \
	chmod u-s bin/su && \
    touch /run/nginx.pid && \
    chown -R cityorde /run/nginx.pid

USER cityorde

COPY my_server.c /home/cityorde/my_server.c
COPY nginx/nginx.conf /etc/nginx/nginx.conf

WORKDIR /home/cityorde/

HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

CMD gcc my_server.c -lfcgi -o my_server && \
    spawn-fcgi -p 8080 ./my_server && \
    nginx -g "daemon off;" && \
    nginx -s reload