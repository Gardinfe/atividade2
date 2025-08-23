# Usar uma imagem base oficial do Python
FROM python:3.9-slim

# Definir o diretório de trabalho dentro do contêiner
WORKDIR /app

# Copiar e instalar as dependências
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copiar o script da aplicação
COPY main.py .

# Expor a porta da aplicação
EXPOSE 5000

# Comando para executar a aplicação
CMD ["python", "app.py"]
