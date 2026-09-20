# Grupo_RASI
Trabalho avaliativo referente ao 3º Bimestre da matéria RASI do ano de  2026 


PRÉ-REQUISITO: Instalar o Docker antes de começar.

# 1. Configure a máquina virtual em modo Bridge. Nas configurações de rede, habilite a placa de rede, selecione “Placa em modo Bridge”, escolha a placa de rede utilizada pelo computador e deixe “Virtual Cable Connected” ativado.

# 2. Inicie o Linux e abra o terminal.

# 3. Descubra o endereço IP da máquina utilizando o comando:

hostname -I

# 4. Anote o endereço IP apresentado. No trabalho, foi utilizado o IP 10.125.131.156.

# 5. A partir da máquina hospedeira, conecte-se ao servidor utilizando o endereço IP encontrado. Na primeira conexão, confirme com “yes” quando solicitado e depois informe a senha.

# 6. Atualize os pacotes do sistema utilizando:

sudo apt update

# 7. Verifique a versão do Docker com:

docker --version

# 8. Teste o funcionamento do Docker com:

docker run hello-world

# 9. Instale a biblioteca tree:

sudo apt install tree

# 10. Crie a pasta principal do projeto:

mkdir projeto-flask

# 11. Crie a pasta app:

mkdir projeto-flask/app

# 12. Crie o Dockerfile:

touch projeto-flask/Dockerfile

# 13. Crie o arquivo app.py:

touch projeto-flask/app/app.py

# 14. Crie o arquivo requirements.txt:

touch projeto-flask/app/requirements.txt

# 15. Visualize a estrutura criada com:

tree projeto-flask/

# 16. Entre na pasta do projeto:

cd projeto-flask

# 17. Abra o Dockerfile:

nano Dockerfile

# 18. Dentro do Dockerfile, coloque:

FROM python:3.14-slim

WORKDIR /app

COPY app/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app/ .

EXPOSE 5000

CMD ["python", "app.py"]

# 19. Salve o Dockerfile e saia do editor.

# 20. Entre na pasta app:

cd app

# 21. Abra o arquivo app.py:

nano app.py

# 22. No arquivo app.py, coloque:

from flask import Flask

app = Flask(__name__)

@app.route("/")
def inicio():
    return """
    <h1>Bem-vindo ao Flask!</h1>
    <p>Esta é a página inicial.</p>

    <a href="/sobre">Sobre</a>
    <a href="/contato">Contato</a>
    """

@app.route("/sobre")
def sobre():
    return """
    <h1>Sobre</h1>
    <p>Este site foi feito com Python e Flask.</p>

    <a href="/">Início</a>
    <a href="/contato">Contato</a>
    """

@app.route("/contato")
def contato():
    return """
    <h1>Contato</h1>
    <p>Email: contato@exemplo.com</p>

    <a href="/">Início</a>
    <a href="/sobre">Sobre</a>
    """

app.run(host="0.0.0.0", port=5000)

# 23. Salve o arquivo app.py e saia do editor.

# 24. Abra o arquivo requirements.txt:

nano requirements.txt

# 25. Coloque dentro dele:

flask

# 26. Salve o arquivo e saia do editor.

# 27. Volte para a pasta principal do projeto:

cd ..

# 28. Construa a imagem Docker com:

docker build -t flask-app .

# 29. Execute o container e faça o mapeamento da porta:

docker run -d -p 5000:5000 --name meu-flask flask-app

# 30. Verifique se o container está funcionando:

docker ps

# 31. Verifique os logs do container:

docker logs meu-flask

# 32. Confira novamente o endereço IP da máquina com:

hostname -I

# 33. Abra um navegador em uma máquina conectada à mesma rede.

# 34. Digite o endereço utilizando o IP da máquina e a porta 5000. No caso do trabalho:

http://10.125.131.156:5000

# 35. Para acessar a página “Sobre”, utilize:

http://10.125.131.156:5000/sobre

# 36. Para acessar a página “Contato”, utilize:

http://10.125.131.156:5000/contato

Ao final, o projeto estará funcionando com Flask dentro de um container Docker. A aplicação poderá ser acessada pelo navegador utilizando o IP da máquina e a porta 5000, e as diferentes páginas poderão ser acessadas pelas rotas /, /sobre e /contato.
lo navegador:

GET / HTTP/1.1 200
GET /favicon.ico HTTP/1.1 404
