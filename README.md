<h1 align="left">Deploy de aplicação containerizada na AWS EC2 com Docker</h1>

###

<h3 align="left">📌 Descrição</h3>

###

<p align="left">Este projeto demonstra o deploy manual de uma aplicação web containerizada em uma instância Amazon EC2, utilizando Docker e Docker Hub como registry.<br>O objetivo foi sair de um estudo puramente teórico sobre Cloud e AWS e aplicar os conceitos na prática, simulando um fluxo real de trabalho de um profissional DevOps/Cloud.</p>

###

<p align="left">Durante o projeto foram aplicados conceitos de:<br>- Containerização com Docker<br>- Construção de imagens<br>- Publicação em registry (Docker Hub)<br>- Provisionamento de instância EC2<br>- Configuração de Security Groups<br>- Acesso remoto via SSH<br>- Troubleshooting de permissões e portas de rede</p>

###

<h3 align="left">🛠️ Tecnologias Utilizadas</h3>

###

<p align="left">- Windows 10<br>- Linux<br>- WSL2<br>- Ubuntu 24.04.4 LTS<br>- Docker Desktop<br>- Docker Hub<br>- Git<br>- AWS EC2<br>- Nginx<br>- SSH</p>

###

<h3 align="left">🎮 Aplicação Utilizada</h3>

###

<p align="left">A aplicação utilizada foi o jogo 2048 baseado no repositório:<br><br>DaoCloud<br>Repositório: https://github.com/DaoCloud/dao-2048<br><br>A aplicação não foi desenvolvida por mim. O foco deste projeto é a infraestrutura e o processo de deploy em ambiente cloud.</p>

###

<h3 align="left">🔧 Etapas do Projeto</h3>

###

<h4 align="left">1 - Configuração do Ambiente Local</h4>

###

<p align="left">- Instalação do WSL2<br>- Instalação do Ubuntu 24.04 LTS<br>- Integração do Docker Desktop com WSL2<br>- Integração do VS Code com ambiente Linux<br><br>wsl --install<br>docker --version</p>

###

<h4 align="left">2- Build da Imagem Docker</h4>

###

<h5 align="left">2.1 - Dockerfile utilizado:</h5>

###

<p align="left">FROM nginx:1.27.3-alpine<br>COPY . /usr/share/nginx/html<br>EXPOSE 80<br>CMD sh /usr/share/nginx/html/scripts/start.sh</p>

###

<h5 align="left">2.2 - Build da imagem:</h5>

###

<p align="left">docker build -t meu-projeto .<br>docker images</p>
<img src="images/01-docker-build.png" width="600"/>

###

<h5 align="left">2.3 - Execução local:</h5>

###

<p align="left">docker run -p 8080:80 meu-projeto</p>

###

<h5 align="left">2.4 - Teste realizado:</h5>

###

<p align="left">http://localhost:8080</p>
<img src="images/02-localhost-running.png" width="600"/>

###

<h4 align="left">3 - Publicação no Docker Hub</h4>

###

<h5 align="left">3.1 - docker login:</h5>

###

<p align="left">docker login</p>

###

<h5 align="left">3.2 - Tag:</h5>

###

<p align="left">docker tag meu-projeto diogo11254/meu-projeto:latest</p>

###

<h5 align="left">3.3 - Push:</h5>

###

<p align="left">docker push diogo11254/meu-projeto:latest</p>
<img src="images/03-dockerhub.png" width="600"/>

###

<h4 align="left">4 - Provisionamento da Instância EC2</h4>

###

<h5 align="left">4.1 - Definições:</h5>

###

<p align="left">- Região: sa-east-1 (São Paulo)<br>- Tipo: t3.micro<br>- Sistema Operacional: Ubuntu Server 24.04 LTS</p>
<img src="images/04-ec2-instance.png" width="600"/>

###

<h5 align="left">4.2 - Security Group:</h5>

###

<p align="left">- Porta 22 liberada apenas para meu IP<br>- Porta 80 liberada<br>- Posteriormente porta 8080 liberada após troubleshooting<br>- Acesso via chave .pem.</p>
<img src="images/05-security-group.png" width="600"/>

###

<h4 align="left">5 - Deploy na EC2</h4>

###

<h5 align="left">5.1 - Instalação do Docker:</h5>

###

<p align="left">sudo apt install docker.io<br>sudo systemctl status docker</p>

###

<h5 align="left">5.2 - Pull da imagem:</h5>

###

<p align="left">sudo docker pull diogo11254/meu-projeto:latest</p>

###

<h5 align="left">5.3 - Execução:</h5>

###

<p align="left">sudo docker run -d -p 8080:80 diogo11254/meu-projeto:latest<br>sudo docker ps</p>
<img src="images/07-docker-ps-ec2.png" width="600"/>

###

<h3 align="left">🧠 Troubleshooting e Aprendizados</h3>

###

<h4 align="left">Problema 1 – Permissões da chave .pem</h4>

###

<p align="left">Erro ao conectar via SSH devido a permissões inadequadas.</p>

###

<h5 align="left">Solução:</h5>
<img src="images/06-ssh-connected.png" width="600"/>

###

<p align="left">chmod 400 arquivo.pem</p>

###

<h5 align="left">Aprendizado:</h5>

###

<p align="left">- Entendimento de permissões Linux<br>- Funcionamento do SSH com chave privada</p>

###

<h4 align="left">Problema 2 – Porta 8080 não acessível</h4>

###

<p align="left">Após executar o container, o acesso via navegador não funcionava.</p>

###

<h5 align="left">Investigação:</h5>

###

<p align="left">- Verificação de logs com docker logs<br>- Container estava funcionando corretamente</p>

###

<h5 align="left">Conclusão:</h5>

###

<p align="left">A porta 8080 não estava liberada no Security Group da EC2.</p>

###

<h5 align="left">Solução:</h5>

###

<p align="left">Criação de regra de entrada liberando a porta 8080 (0.0.0.0/0).</p>

###

<h5 align="left">Aprendizado:</h5>

###

<p align="left">- Entendimento prático de Security Groups<br>- Conceito de portas expostas vs portas liberadas na nuvem</p>

###

<h3 align="left">📱 Resultado Final</h3>

###

<p align="left">Aplicação acessível via IP público da EC2.</p>

###

<h4 align="left">Teste realizado com sucesso:</h4>

###

<img src="images/08-mobile-access.png" width="250"/>

###

<h3 align="left">📚 Principais Conceitos Fixados</h3>

###

<p align="left">- Fluxo completo de deploy manual<br>- Diferença entre container e instância<br>- Funcionamento do Docker Hub como registry<br>- Segurança básica em Cloud<br>- Permissões no Linux<br>- Mapeamento de portas<br>- Troubleshooting em ambiente real</p>

###

<div align="left">
  <img src="https://skillicons.dev/icons?i=docker" height="40" alt="docker logo"  />
  <img width="12" />
  <img src="https://skillicons.dev/icons?i=aws" height="40" alt="amazonwebservices logo"  />
  <img width="12" />
  <img src="https://skillicons.dev/icons?i=linux" height="40" alt="linux logo"  />
  <img width="12" />
  <img src="https://skillicons.dev/icons?i=git" height="40" alt="git logo"  />
</div>

###
