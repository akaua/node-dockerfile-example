# soft-test

## Perguntas e respostas:

### A aplicação deverá rodar em Docker, com o menor tamanho de imagem possível.

#### R: Utilizei imagem alpine e levei somente o necessario para rodar a aplicacao.

### Precisará estar pronta para escalar, mesmo que seja manualmente.

#### R: No contexto de aplicacao eh possivel utilizar um gerenciado como PM2. Porem utilizando docker e uma ferramenta de orquestracao como K8S, Docker Swarm ou ECS(AWS) e um conjunto de recursos da Amazon como *auto scaling group* e ALB eh possivel deixar a aplicacao pronta para escalar horizontalmente de maneira automatica.

### A aplicação necessita de um proxy reverso intermediando as conexões HTTP e HTTPS para a aplicação.

#### R: Utilizaria Nginx como proxy reverso, porem dependendo da necessidade de um ALB repassando as requisicoes para o proxy o Certificado SSL ficaria dentro do ALB (Se possivel utilizando um gerenciador de certificados como *ACM* da Amazon). Caso nao utilize um ALB, utilizaria o certificado diretamente no Nginx.

### Deverá ser configurada uma política de deploy e rollback, em caso de falha no deploy da nova versão a versão atual deverá ser mantida.

#### R: Utilizaria uma estrategia de deploy blue/green, assim seria feito a alternancia de ambientes em caso de falha no deploy. Falando de banco de dados eh possivel utilizar uma ferramenta de migracao como db-migrate.

### Precisará ser monitorada proativamente com envio de alertas em caso de anormalidades.

#### R: Utilizaria prometheus e grafana configurados por ansible para o monitoramento ativo, faria as verificacoes basicas nos containers e servidores (memoria,cpu,disco e etc) e tambem nos status de resposta das aplicacoes (4xx,5xx). O envio de alerta seria feito pelo canal de preferencia (gosto muito do Telegram para o caso).

### Deverá ser criado um processo de deploy automatizado para cada versão nova dessa aplicação.

#### R: Utilizaria Jenkins configurado por ansible e jobs configurados por groovy, onde os pipelines ficam configurados dentro dos Jenkinsfile de cada projeto. Dependendo da branch o deploy sera feito para determinado ambiente (dev,qa,staging) e para producao devera ter aceite manual do pipeline para seguranca ( Dentro do pipeline eh importante existir tantos barreiras de qualidade quanto possivel, como testes unitarios, qualidade de codigo, escaneamento de vulnerabilidade, teste de carga e teste de funcionalidade com Selenium por exemplo, porem eh preciso ver o tempo desses testes, pode ser necessario executa-los em outro momento e nao dentro do pipeline).

### Os logs deverão ser enviados para um centralizador.

#### Utilizaria um centralizador como Graylog ou a stack ELK (Elasticsearch,logstash,kibana). AWS cloudwatch tambem pode ser bem util, principalmente se for utilizado um projeto para leitura local desses logs como aws_logs *https://github.com/jorgebastida/awslogs*
