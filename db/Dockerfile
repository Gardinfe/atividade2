# Usar a imagem base oficial do PostgreSQL
FROM postgres:13

# Copiar o script de inicialização para o diretório correto.
# A imagem do Postgres executa automaticamente qualquer .sql neste diretório.
COPY init.sql /docker-entrypoint-initdb.d/
