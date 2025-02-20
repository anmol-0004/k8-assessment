FROM debian:bullseye AS apache

# Install Apache, Node.js, and dependencies
RUN apt-get update && apt-get install -y \
    apache2 apache2-utils curl \
    && curl -fsSL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs \
    && apt-get clean

# Set working directory
WORKDIR /usr/src/app

# Copy package.json and install dependencies
COPY package.json ./
RUN npm install --only=production && npm cache clean --force

# Copy application files
COPY . .

# Enable Apache modules
RUN a2enmod proxy proxy_http rewrite

# Copy custom Apache configuration
COPY httpd.conf /etc/apache2/sites-available/000-default.conf

# Expose port 80 for Apache
EXPOSE 80

# Start Node.js and Apache
CMD ["sh", "-c", "node /usr/src/app/server.js & apachectl -D FOREGROUND"]
