FROM joomla:php7.3-fpm
MAINTAINER Prime-Host <info@nordloh-webdesign.de>

ARG DEBIAN_FRONTEND=noninteractive

# install basics
RUN mkdir /var/run/sshd /var/log/php-fpm \
 && apt-get update \
 && apt-get install -y cron python-setuptools wget curl git nano vim sudo unzip \ 
    openssh-server openssl zsh supervisor mariadb-client

RUN wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true \
 && cp /root/.oh-my-zsh/themes/bira.zsh-theme /root/.oh-my-zsh/themes/prime-host.zsh-theme \
 && sed -i 's/%m/%M/g' /root/.oh-my-zsh/themes/prime-host.zsh-theme \
 && sed -i s:/root/.oh-my-zsh:\$HOME/.oh-my-zsh:g /root/.zshrc \
 && sed -i 's/robbyrussell/prime-host/g' /root/.zshrc \
 && echo "DISABLE_UPDATE_PROMPT=true" >> /root/.zshrc \
 && echo "set encoding=utf-8" >> /root/.vimrc \
 && echo "set fileencoding=utf-8" >> /root/.vimrc \
 && echo "cd /var/www/html" >> /root/.zshrc \
 && cp -r /root/.oh-my-zsh /etc/skel/. \
 && cp /root/.zshrc /etc/skel/. \
 && cp /root/.vimrc /etc/skel/. \
 && mkdir /root/container-scripts /root/container-scripts/prime-host /root/container-scripts/custom \
 && sed -i "s|access.log = /proc/self/fd/2|access.log = /proc/self/fd/1|g" /usr/local/etc/php-fpm.d/docker.conf \
 && sed -i 's/pm.max_children = 5/pm.max_children = 100/g' /usr/local/etc/php-fpm.d/www.conf \
 && sed -i 's/pm.start_servers = 2/pm.start_servers = 10/g' /usr/local/etc/php-fpm.d/www.conf \
 && sed -i 's/pm.min_spare_servers = 1/pm.min_spare_servers = 5/g' /usr/local/etc/php-fpm.d/www.conf \
 && sed -i 's/pm.max_spare_servers = 3/pm.max_spare_servers = 20/g' /usr/local/etc/php-fpm.d/www.conf \ 
 && sed -i 's/;pm.max_requests = 500/pm.max_requests = 500/g' /usr/local/etc/php-fpm.d/www.conf \
 && apt-get --purge autoremove -y 

ADD ./joomla.ini /usr/local/etc/php/conf.d/joomla.ini
ADD ./supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ADD ./start.sh /bin/prime-host/start.sh

WORKDIR /var/www/html

CMD ["/bin/bash", "/bin/prime-host/start.sh"]
