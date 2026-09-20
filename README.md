# Grupo_RASI
Trabalho avaliativo referente ao 3º Bimestre da matéria RASI do ano de  2026 
Grupo: Gabriel Rizzo, Melissa e Mariah.

PRÉ-REQUISITO: Instalar o Docker antes de começar.

# 1 Configure a máquina virtual em modo Bridge. Nas configurações de rede, habilite a placa de rede, selecione "Placa em modo Bridge", escolha a placa de rede utilizada pelo computador e deixe "Virtual Cable Connected" ativado.
Explicação: O modo Bridge faz a VM aparecer na rede como um dispositivo independente, recebendo seu próprio IP e ficando acessível por outras máquinas.

# 2 Inicie o Linux e abra o terminal.
Explicação: Inicialização do sistema operacional e abertura do terminal para executar os comandos.

# 3 Descubra o endereço IP da máquina utilizando o comando:
hostname -I
Explicação: hostname -I exibe todos os endereços IP da máquina. É essencial para saber onde a aplicação estará acessível.

# 4 Anote o endereço IP apresentado. No trabalho, foi utilizado o IP 10.125.131.156.
Explicação: Guardar o IP para usá-lo posteriormente ao acessar a aplicação pelo navegador.

# 5 A partir da máquina hospedeira, conecte-se ao servidor utilizando o endereço IP encontrado. Na primeira conexão, confirme com "yes" quando solicitado e depois informe a senha.
Explicação: Conexão remota (geralmente via SSH) da máquina física à VM, permitindo trabalhar nela de fora.

# 6 Atualize os pacotes do sistema utilizando:
sudo apt update
Explicação: Atualiza a lista de pacotes disponíveis nos repositórios do sistema, garantindo que as próximas instalações usem as versões mais recentes.

# 7 Verifique a versão do Docker com:
docker --version
Explicação: Mostra a versão do Docker instalada, confirmando que ele está corretamente instalado.

# 8 Teste o funcionamento do Docker com:
docker run hello-world
Explicação: Baixa e executa um container de teste. Se aparecer a mensagem "Hello from Docker!", a instalação está funcionando.

# 9 Instale a biblioteca tree:
sudo apt install tree
Explicação: Instala o utilitário tree, que exibe a estrutura de diretórios em formato de árvore.

# 10 Crie a pasta principal do projeto:
mkdir projeto-flask
Explicação: Cria o diretório raiz do projeto Flask.

# 11 Crie a pasta app:
mkdir projeto-flask/app
Explicação: Cria a subpasta app, que conterá o código-fonte da aplicação.

# 12 Crie o Dockerfile:
touch projeto-flask/Dockerfile
Explicação: Cria um arquivo vazio chamado Dockerfile, que definirá como a imagem Docker será construída.

# 13 Crie o arquivo app.py:
touch projeto-flask/app/app.py
Explicação: Cria o arquivo principal da aplicação Flask.

# 14 Crie o arquivo requirements.txt:
touch projeto-flask/app/requirements.txt
Explicação: Cria o arquivo que listará as dependências Python do projeto.

# 15 Visualize a estrutura criada com:
tree projeto-flask/
Explicação: Mostra graficamente a estrutura de pastas e arquivos criados até aqui.

# 16 Entre na pasta do projeto:
cd projeto-flask
Explicação: Muda o diretório atual para a pasta do projeto.

# 17 Abra o Dockerfile:
nano Dockerfile
Explicação: Abre o Dockerfile no editor de texto nano para edição.

# 18 Dentro do Dockerfile, coloque:

FROM python:3.14-slim

WORKDIR /app

COPY app/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app/ .

EXPOSE 5000

CMD ["python", "app.py"]

Explicação:
- FROM python:3.14-slim -> usa a imagem base do Python 3.14 (versão leve).
- WORKDIR /app -> define o diretório de trabalho dentro do container.
- COPY app/requirements.txt . -> copia o requirements.txt para o container.
- RUN pip install... -> instala as dependências listadas.
- COPY app/ . -> copia o restante do código da aplicação.
- EXPOSE 5000 -> informa que a aplicação escuta na porta 5000.
- CMD [...] -> comando executado ao iniciar o container.

# 19 Salve o Dockerfile e saia do editor.
Explicação: No nano, salva com Ctrl+O e sai com Ctrl+X.

# 20 Entre na pasta app:
cd app
Explicação: Entra na pasta onde está o código da aplicação.

# 21 Abra o arquivo app.py:
nano app.py
Explicação: Abre o arquivo app.py no editor nano.

# 22 No arquivo app.py, coloque:

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

Explicação: Define três rotas (/, /sobre, /contato) que retornam páginas HTML, e executa o servidor Flask em todas as interfaces (0.0.0.0) na porta 5000.

# 23 Salve o arquivo app.py e saia do editor.
Explicação: Ctrl+O para salvar e Ctrl+X para sair.

# 24 Abra o arquivo requirements.txt:
nano requirements.txt
Explicação: Abre o arquivo de dependências no nano.

# 25 Coloque dentro dele:
flask
Explicação: Lista a biblioteca Flask como dependência a ser instalada no container.

# 26 Salve o arquivo e saia do editor.
Explicação: Salvar e fechar o nano.

# 27 Volte para a pasta principal do projeto:
cd ..
Explicação: Sobe um nível na árvore de diretórios, voltando para projeto-flask.

# 28 Construa a imagem Docker com:
docker build -t flask-app .
Explicação: Constrói a imagem Docker a partir do Dockerfile atual, nomeando-a flask-app. O "." indica o diretório atual como contexto.

# 29 Execute o container e faça o mapeamento da porta:
docker run -d -p 5000:5000 --name meu-flask flask-app
Explicação:
- -d -> executa em segundo plano (detached).
- -p 5000:5000 -> mapeia a porta 5000 do host para a 5000 do container.
- --name meu-flask -> nomeia o container.
- flask-app -> imagem usada.

# 30 Verifique se o container está funcionando:
docker ps
Explicação: Lista os containers em execução, mostrando nome, imagem, portas e status.

# 31 Verifique os logs do container:
docker logs meu-flask
Explicação: Exibe a saída do container, útil para depurar erros ou ver mensagens do Flask.

# 32 Confira novamente o endereço IP da máquina com:
hostname -I
Explicação: Confirma o IP atual da VM para acessar a aplicação pelo navegador.

# 33 Abra um navegador em uma máquina conectada à mesma rede.
Explicação: O acesso deve ser feito de outro dispositivo na mesma rede para testar a aplicação.

# 34 Digite o endereço utilizando o IP da máquina e a porta 5000. No caso do trabalho:
http://10.125.131.156:5000
Explicação: Acessa a página inicial (/) da aplicação Flask rodando no container.

# 35 Para acessar a página "Sobre", utilize:
http://10.125.131.156:5000/sobre
Explicação: Acessa a rota /sobre definida no app.py.

# 36 Para acessar a página "Contato", utilize:
http://10.125.131.156:5000/contato
Explicação: Acessa a rota /contato definida no app.py.

Ao final, o projeto estará funcionando com Flask dentro de um container Docker. A aplicação poderá ser acessada pelo navegador utilizando o IP da máquina e a porta 5000, e as diferentes páginas poderão ser acessadas pelas rotas /, /sobre e /contato.
lo navegador:

GET / HTTP/1.1 200
GET /favicon.ico HTTP/1.1 404
