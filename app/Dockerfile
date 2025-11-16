# Utiliser une image Node.js officielle
FROM node:20-alpine

# Créer le dossier de l'application dans le container
WORKDIR /usr/src/app

# Copier les fichiers package.json et package-lock.json
COPY package*.json ./

# Installer les dépendances
RUN npm install

# Copier le reste des fichiers de l'application
COPY . .

# Exposer le port 80
EXPOSE 80

# Lancer l'application
CMD ["node", "index.mjs"]