# Tutorial de Implementação

### Sistema Docker Swarm com Grafana, MySQL, WordPress e Prometheus

1. <b>Preparação</b>
   <br>
   Certifique-se de ter o Docker, Git e WSL (se estiver no Windows) instalados.
   <br>
   Verifique se as portas necessárias não estão em uso.
   <br><br>
2. <b>Criação do Diretório</b>
   <br>
   Crie uma pasta e entre nela e clone o repositório:

```
mkdir Pasta
cd Pasta
git clone https://github.com/samuelt005/trabalho-docker-final.git
cd TrabalhoDocker
```

3. <b>Execução dos Comandos</b>
   <br>
   Execute os seguintes comandos:

```
docker stack deploy -c docker-compose.yml wordpress_stack
docker node ls
docker swarm init
docker node inspect -f '{{ .Status.Addr }}' NomeDoHost
docker ps
docker exec -it <ID do container do WordPress> bash
apt update
apt install nano
nano wp-config.php
```

No arquivo wp-config.php, adicione os seguintes comandos entre as linhas:

```
if ($configExtra = getenv_docker('WORDPRESS_CONFIG_EXTRA', '')) {
    eval($configExtra);
}

/* That's all, stop editing! Happy publishing. */
define('WP_CACHE', true);
define('WP_REDIS_HOST', 'redis');
define('WP_REDIS_PORT', 6379);
```

Salve e saia do editor (Ctrl+X, Y, Enter).

4. <b>Verificação do MySQL</b>
   <br>
   Ainda dentro do container do WordPress, execute:

```
apt install default-mysql-client
mysql -h db -u wpuser -pwppassword wordpress
show tables;
Ctrl+C
exit
```

5. <b>Acesso ao WordPress</b>
   <br>
   Acesse: <a> http://\<IP_da_maquina_manager>:80 </a>
   <br>
   Você deve ver a tela do WordPress.
   <br>
   Acesse: <a> http://\<IP_da_maquina_manager>:80/admin </a>
   <br>
   Vá em "Ferramentas" > "Info" e procure por "Database".
   As configurações do banco de dados devem ser exibidas.
   Vá em "Plugins" > "Adicionar Novo Plugin", procure por "Redis", instale e ative.
   <br>
   Depois vá em "Configurações" > "Redis" e inicie o Redis.
   <br> <br>
6. <b>Acesso ao Prometheus</b>
   <br>
   Acesse: <a>http://\<IP_da_maquina_manager>:9090</a>
   <br>
   Na aba "Expressions", digite "up" e clique em "Execute".
   <br>
   Verifique as instâncias em execução.
   <br> <br>
7. <b>Acesso ao Grafana</b>
   <br>
   Acesse: <a>http://\<IP_da_maquina_manager>:3000 </a>
   <br>
   Faça login com as credenciais "Admin" e "Admin".
   <br>
   Vá em "Data Sources".
   <br>
   "Add new data source".
   <br>
   Selecione "Prometheus".
   <br>
   Na aba de conexão, insira <a> http://\<IP_da_maquina_manager>:9090 </a>
   <br>
   Clique em "Save & Test".
   <br>
   <br>
8. <b>Importação de Dashboard </b>
   <br>
   Vá em "Dashboards" > "New" > "Import".
   <br>
   Importe um código de dashboard, como, por exemplo '3662', e clique em "Import".
   <br>
   <br>
9. <b>Verificação de Aplicações</b>
   <br>
   Acesse: <a>http://\<IP_da_maquina_manager>:9090/Targets</a>
   <br>
   Verifique se as aplicações estão rodando com sucesso.

   <br>
   <br>
   <i>Samuel Andrei Horn Thomas</i>
