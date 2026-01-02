# Start from the official code-server image
FROM codercom/code-server:latest

# Use a non-root user for installing packages
USER root

# Install Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
  apt-get install -y nodejs

RUN npm i --global yarn

# Install PHP 8.3
RUN apt-get update \
  && mkdir -p /etc/apt/keyrings \
  && apt-get install -y gnupg gosu curl ca-certificates zip unzip git supervisor sqlite3 libcap2-bin libpng-dev dnsutils librsvg2-bin fswatch ffmpeg nano  \
  && curl -sS 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x14aa40ec0831756756d7f66c4f4ea0aae5267a6c' | gpg --dearmor | tee /etc/apt/keyrings/ppa_ondrej_php.gpg > /dev/null \
  && echo "deb [signed-by=/etc/apt/keyrings/ppa_ondrej_php.gpg] https://ppa.launchpadcontent.net/ondrej/php/ubuntu jammy main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
  && apt-get update \
  && apt-get install -y php8.3-cli php8.3-dev \
  php8.3-pgsql php8.3-sqlite3 php8.3-gd \
  php8.3-curl \
  php8.3-imap php8.3-mysql php8.3-mbstring \
  php8.3-xml php8.3-zip php8.3-bcmath php8.3-soap \
  php8.3-readline \
  php8.3-ldap \
  php8.3-msgpack php8.3-igbinary php8.3-redis php8.3-swoole \
  php8.3-memcached php8.3-pcov php8.3-imagick php8.3-xdebug \
  && curl -sLS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

# Remove coder from the sudo group
RUN deluser coder sudo || true

# Switch back to the coder user
USER coder

# Set the working directory to the home directory of 'coder'
WORKDIR /home/coder

# Expose the code-server port
EXPOSE 8080

# Start code-server
CMD ["code-server", "--bind-addr", "0.0.0.0:8080"]
