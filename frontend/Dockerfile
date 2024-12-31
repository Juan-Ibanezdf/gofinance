# Etapa de Build
FROM node:18-alpine AS builder

# Diretório de trabalho
WORKDIR /frontend

# Copiar arquivos necessários para instalar dependências
COPY package.json package-lock.json ./

# Instalar dependências
RUN npm install

# Copiar o restante do código do frontend
COPY . .

# Build da aplicação
RUN npm run build

# Etapa de Produção
FROM node:18-alpine

# Diretório de trabalho
WORKDIR /frontend

# Instalar dependências de produção
COPY package.json package-lock.json ./
RUN npm install --production

# Copiar o build do frontend
COPY --from=builder /frontend/.next ./.next
COPY public ./public
COPY src ./src

# Expor a porta do frontend
EXPOSE 3000

# Comando padrão
CMD ["npm", "run", "dev"]
