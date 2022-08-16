FROM amazonlinux:2
# default install
ARG USERNAME=ec2-user
RUN yum update -y \
	&& yum install \
	shadow-utils \
	systemd \
	gcc \
	gcc-c++ \
	glibc \
	make \
	openssl-devel \
	tar \
	unzip \
	sudo \
	git \
	wget \
	-y

RUN yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN yum -y install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
RUN yum -y install yum-utils
RUN yum-config-manager --disable 'remi-php*'
RUN yum-config-manager --enable remi-php81
RUN yum -y install php81
RUN yum -y install php81-php-bcmath php81-php-mbstring php81-php-pdo php-tokenizer php81-php-xml php81-php-mysqlnd php81-php-gd php81-php-pecl-zip php81-php-pecl-xdebug3 php81-php-opcache php81-php-pecl-apcu
RUN alternatives --install /usr/bin/php php /usr/bin/php81 1


# composer install
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# create user
RUN useradd "ec2-user" && echo "ec2-user ALL=NOPASSWD: ALL" >> /etc/sudoers

#php-cs-fixer formatter
RUN su ec2-user -c 'composer global require "friendsofphp/php-cs-fixer"'

# nvm
RUN su ec2-user -c 'curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash'
RUN su ec2-user -c '. ~/.nvm/nvm.sh'
# install node 17.9 
# glibc のバージョンの関係で18は入らない
RUN su - ec2-user -c 'nvm install 17.9.0'