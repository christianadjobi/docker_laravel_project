# Utiliser l'image Ubuntu comme base
FROM ubuntu:latest

# Définir des variables d'environnement pour éviter les questions d'installation interactives
ENV DEBIAN_FRONTEND=noninteractive

# Mettre à jour l'image et installer les dépendances nécessaires
RUN apt-get update && \
    apt-get install -y \
    software-properties-common \
    curl \
    unzip \
    && add-apt-repository ppa:ondrej/php -y

# Installer PHP 8.2 et quelques extensions courantes
RUN apt-get update && \
    apt-get install -y \
    php8.2 \
    php8.2-cli \
    php8.2-mbstring \
    php8.2-xml \
    php8.2-curl \
    php8.2-zip   \
    php8.2-bcmath \
    php8.2-sqlite3 \
    git

# Installer Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Vérifier les installations de PHP et de Composer
RUN php -v && composer -V

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers de l'application dans le conteneur
COPY . /app

RUN cp .env.example .env
# Installer les dépendances Composer
RUN composer install --no-interaction --prefer-dist

RUN php artisan key:generate

# Exécuter les migrations (après installation de Composer)
RUN php artisan migrate --force


# Commande par défaut
CMD ["php", "artisan", "serve", "--host", "0.0.0.0", "--port", "8000"]

