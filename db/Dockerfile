# Usar a imagem base oficial do PostgreSQL
FROM postgres:13

# Copiar o script de inicialização para o diretório correto.
# O nome do arquivo foi corrigido para corresponder ao seu repositório.
COPY codigo.sql /docker-entrypoint-initdb.d/
