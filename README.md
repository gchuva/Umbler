# Umbler Pergunta 1
teste processo seletivo Umbler

# Quais comandos você usaria para diagnosticar o problema? (logs, status de serviços, portas em uso, processos.)

systemctl status nginx (Verifica status do serviço Nginx)
systemctl status php-fpm ( Verifica status do serviço php-fpm)

# Verificar logs do Nginx
tail -f /var/log/nginx/error.log
tail -f /var/log/nginx/access.log

# Verificar logs do PHP-FPM
tail -f /var/log/php-fpm/error.log

# Verificar portas
netstat -tulpn | grep 9000  # porta padrão do PHP-FPM

netstat -tulpn | grep 80 

netstat -tulpn | grep 443

# Verificar processos
ps aux | grep -i nginx

ps aux | grep -i php-fpm

3 causas prováveis:

# Causa 1

PHP-FPM parado ou apresentando falha

Passo de verificação

systemctl status php-fpm

# Se estiver inativo:

systemctl start php-fpm

systemctl enable php-fpm

# Causa 2

# Problemas com Socket

# Verificar se o socket existe
ls -larth /var/run/php/php*-fpm.sock

# Verificar permissões (precisa ser pelo usuário do Nginx)

# Verificar configuração do pool PHP-FPM
cat /etc/php-fpm.d/www.conf | grep listen

# Obs: O Socket pode desaparecer se o processo parar ou as permissões estiverem incorretas.

# Causa 3

# PHP-FPM Sobrecarregado

# Verificação

# Verificar processos PHP-FPM ativos
ps aux | grep -i php-fpm | wc -l

# Como diferenciar o problema

# Problema no servidor Web

systemctl status nginx # Verifica se o serviço do Nginx esta respondendo.

curl -I http://localhost # o mesmo deve retornar 502 que seria o erro ao qual estamos tratando.

netstat -tulpn | grep nginx # para verificar se as portas 80 e 443 estão respondendo

# Problemas na aplicação PHP-FPM

systemctl status php-fpm  # Verifica se o serviço esta rodando.

verificar logs da aplicação PHP-FPM para identifcar possíveis erros.

# Problemas de rede

Firewall pode estar bloqueando as portas.
iptables -L -n ou ufw status # para verificar informações do FW

Teste de conexão telnet 127.0.0.1 9000


