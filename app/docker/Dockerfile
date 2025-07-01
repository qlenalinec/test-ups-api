# Utilise une image officielle PHP 5.6 avec Apache
FROM php:5.6-apache-stretch

#[Debian 9/stretch]The stretch suites have been imported in archive.debian.org
#Need to update stretch repositories before update
RUN sed -i s/deb.debian.org/archive.debian.org/g /etc/apt/sources.list
RUN sed -i 's|security.debian.org|archive.debian.org/|g' /etc/apt/sources.list
RUN sed -i '/stretch-updates/d' /etc/apt/sources.list

# Installe les dépendances nécessaires
RUN apt-get update && apt-get install -yq --no-install-recommends --allow-unauthenticated \
    git \
    unzip \
    zip \
    libxml2-dev

RUN docker-php-ext-install xml

# Active mod_rewrite pour Apache (si nécessaire)
RUN a2enmod rewrite

# Installe Composer
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer
# Copie les fichiers du projet dans le conteneur
COPY . /var/www/html

# Définit le répertoire de travail
WORKDIR /var/www/html

# Installe les dépendances PHP via Composer
RUN composer update --no-interaction --prefer-dist
RUN composer install --no-interaction --prefer-dist

# Commande par défaut : exécute les tests PHPUnit
CMD ["vendor/bin/phpunit", "--verbose"]
