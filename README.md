# yaman_pdi1
Repositório para versionar o meu primeiro PDI na Yaman 

# Objetivo
1. Fornecer ambiente de laboratório para estudos de testes não funcionais que estimulem a implantação de infraestrutura através de containers, execução de testes em ambiente controlado com total autonomia sobre todos os componentes do ecossistema envolvido. <br>
2. Estimular o compartilhamento de informações e recursos entre os pares a fim de obter aprimoramento técnico e enriquecer a cultura de troca de conhecimentos. <br>

# Requisitos
1. Docker e Docker compose instalados e configurados. (em ambiente Windows, Docker desktop é suficiente pois já traz o Docker compose nativamente).
2. Navegador.
3. Jmeter.

# Como executar
1. Clonar o projeto do github: git clone https://github.com/devrcapelini/yaman_pdi1.git
2. Navegar até a pasta do projeto que contém o arquivo docker-compose.yaml
3. Executar o comando docker-compose up.
   - Garantir que as portas tcp 80 e 9090 estejam disponíveis para a execução

# Verificando se o ambiente está no ar
1. Através do comando docker ps, verificar se os containers estão ativos:
- rcapelini@thehost:~$ docker ps <br>

| CONTAINER ID 	| IMAGE                                  	| COMMAND                	| CREATED      	| STATUS                  	| PORTS                                     	| NAMES                	|
|--------------	|----------------------------------------	|------------------------	|--------------	|-------------------------	|-------------------------------------------	|----------------------	|
| 161e842ca4c4 	| nginx                                  	| "/docker-entrypoint.…" 	| 2 months ago 	| Up 52 seconds           	| 0.0.0.0:80->80/tcp, :::80->80/tcp         	| proxy-forum-api      	|
| 6953271c81e4 	| prom/prometheus:v2.45.0                	| "/bin/prometheus --c…" 	| 2 months ago 	| Up 47 seconds           	| 0.0.0.0:9090->9090/tcp, :::9090->9090/tcp 	| prometheus-forum-api 	|
| 5132602b2580 	| devrcapelini/app_curl-forum-api:latest 	| "java -Xms128M -Xmx1…" 	| 2 months ago 	| Up 57 seconds (healthy) 	|                                           	| app-forum-api        	|
| 6b7adc6ee559 	| mysql:5.7                              	| "docker-entrypoint.s…" 	| 2 months ago 	| Up 59 seconds           	|                                           	| mysql-forum-api      	|
| 84e42dd8ab93 	| redis                                  	| "docker-entrypoint.s…" 	| 2 months ago 	| Up About a minute       	|                                           	| redis-forum-api      	|

------------------------------------------------------------------------

2. Acessando o endereço http://127.0.0.1/health


# Métricas disponíveis
### O Micrometer disponibiliza algumas métricas da JVM por padrão que são muito úteis em uma análise de performance.
- As métricas disponíveis podem ser consultadas aqui: http://127.0.0.1/metrics

## Métricas da JVM
- Irei descrever as métricas da JVM em atualizações deste documento, por agora, é possível ter uma visão sobre as métricas expostas aqui:
- https://spring.io/blog/2018/03/16/micrometer-spring-boot-2-s-new-application-metrics-collector


## Métricas Personalizadas
### As métricas personalizadas são geradas através de implementação, utilizando as classes do micrometer na aplicação, futuramente incluirei uma explicação sobre a implementação, mas por agora é suficiente saber que as métricas que criamos até o momento neste projeto são:
- auth_user_success_total
- auth_user_error_total



# Consultando o Prometheus
- para consultar a tela do Prometheus, basta acessar o endereço local http://127.0.0.1:9090
- A documentação sobre consultas no Prometheus pode ser lida aqui: https://prometheus.io/docs/prometheus/latest/querying/basics/
- No campo de consulta, podemos utilizar para a análise as métricas disponíveis em http://127.0.0.1/metrics
- A linguagem de consulta utilizada no prometheus é a PromQL que pode ser consultada na documentação.
  

# Rodando um robô simples e verificando as métricas
- Ao acessar a tela de consulta do prometheus, como exemplo, consulte as métricas:
   - auth_user_success_total
   - auth_user_error_total
   
- Rodar o robô auth.jmx incluso na pasta do projeto através do comando:
   - jmeter -n -t auth.jmx

- Será possível observar o aumento de ambas as ocorrências devido a forma como o robô foi contruído

# To-do list
- Enriquecer esta documentação com mais explicações e aplicabilidades
- Incluir o Grafana na stack
- Incluir o AlertManager
- Criar a automação com Jmeter + Selenium para demonstar uma utilização específica do Jmeter
- Implementar endpoints de inclusão de dados
- Implementar endpoints de consultas mais onerosos para visualizar o consumo de recursos computacionais através da observabilidade
