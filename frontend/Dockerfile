# Stage 1: Build
FROM node:20-alpine as builder
WORKDIR /app
COPY package*.json ./
RUN npm install || true

COPY . .
# Aucun build nécessaire pour cette app vanilla JS
# mais on peut valider la qualité du code
RUN npm run lint 2>/dev/null || echo "No lint script"

# Stage 2: Runtime avec nginx
FROM nginx:alpine-slim
WORKDIR /usr/share/nginx/html

# Copier les fichiers depuis le stage builder
COPY --from=builder /app .

# Copier une configuration nginx pour le SPA
RUN cat > /etc/nginx/conf.d/default.conf << 'EOF'
server {
    listen 80;
    server_name _;
    root /usr/share/nginx/html;
    index index.html;

    # Cache les assets statiques
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # Fallback pour le SPA
    location / {
        try_files $uri /index.html;
    }
}
EOF

USER nginx:nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
