# Etapa de Build
FROM golang:1.23.4-alpine AS builder

# Instalar dependências necessárias
RUN apk add --no-cache git bash curl gcc musl-dev

# Diretório de trabalho no backend
WORKDIR /backend

# Copiar apenas os arquivos do módulo Go
COPY go.mod go.sum ./
RUN go mod tidy

# Copiar o restante do código do backend
COPY . .

# Instalar Air para hot-reload
RUN go install github.com/air-verse/air@latest

# Etapa Final
FROM golang:1.23.4-alpine

# Instalar ferramentas mínimas e o golang-migrate
RUN apk add --no-cache bash curl make && \
    curl -L https://github.com/golang-migrate/migrate/releases/download/v4.15.2/migrate.linux-amd64.tar.gz | tar xvz && \
    mv migrate /usr/local/bin/

# Diretório de trabalho no backend
WORKDIR /backend

# Copiar o backend, o Air e o Makefile
COPY --from=builder /backend/ .
COPY --from=builder /go/bin/air /usr/local/bin/air

# Expor a porta do backend
EXPOSE ${BACKEND_PORT}

# Configurar o comando padrão para executar o Air
CMD ["air"]
