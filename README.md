# zabbix_upgrade
zabbix_upgrade 6.4 to 7.0
#
CRIAR AS PASTAS PARA O BACKUP

mkdir -p /mnt/bkp_zabbix/etc_zabbix/zabbix_conf_files;<BR>
mkdir -p /mnt/bkp_zabbix/usr_sbin/zabbix_server_files;<BR>
mkdir -p /mnt/bkp_zabbix/usr_share_doc/zabbix_files;<BR>
mkdir -p /mnt/bkp_zabbix/etc_nginx/conf_files;<BR>
mkdir -p /mnt/bkp_zabbix/etc_nginx/conf_d/conf_files;<BR>
mkdir -p /mnt/bkp_zabbix/php_fpm_d/zabbix_conf_files;<BR>
mkdir -p /mnt/bkp_zabbix/zabbixbd/bdzabbix_files;<BR>

-COPIAR OS BACKUP´S

cp -rp /etc/zabbix//mnt/bkp_zabbix/etc_zabbix/zabbix_conf_files;<BR>
cp -rp /usr/sbin/zabbix/mnt/bkp_zabbix/usr_sbin/zabbix_server_files;<BR>
cp -rp /usr/share/doc/zabbix-*/mnt/bkp_zabbix/usr_share_doc/zabbix_files;<BR>
cp -rp /etc/nginx/nginx.conf/mnt/bkp_zabbix/etc_nginx/conf_files;<BR>
cp -rp /etc/nginx/conf.d/zabbix.conf//mnt/bkp_zabbix/etc_nginx/conf_d/conf_files;<BR>
cp -rp /etc/php-fpm.d/mnt/bkp_zabbix/php_fpm_d/zabbix_conf_files;<BR>

-PARAR O SERVIÇO DO ZABBIX

systemctl stop zabbix-server

-VER A SENHA DO MYSQL

egrep -v '^$|^#' /etc/zabbix/zabbix_server.conf

-ACESSAR O MYSQL E ATIVAR log_bin_trust PARA ALTERAR AS TABELAS DO BANCO DURANTE A ATUALIZAÇÃO.

mysql -uroot -p
password mysql> set global log_bin_trust_function_creators = 1; mysql> quit;

-VALIDAR VERSÃO LINUX cat /etc/redhat-release

-VALIDAR VERSÃO ZABBIX INSTALADO rpm -qa | grep zabbix

-ACESSAR A PAGINA DA ZABBIX PARA PEGAR O RELEASE DO S.O. https://www.zabbix.com/download

-INSTALAR O NOVO RELEAsE (exemplo: rocky linux 9) rpm -Uvh https://repo.zabbix.com/zabbix/6.5/rocky/9/x86_64/zabbix-release-6.5-2.el9.noarch.rpm dnf clean all

-ATUALIZAR OS PACOTES DO ZABBIX PARA NOVA VERSÃO dnf update -y

-ATUALIZAR O BANCO DE DADOS PARA A NOVA VERSÃO systemctl start zabbix-server && tail -f /var/log/zabbix/zabbix_server.log

-PARAR O SERVIÇO DO ZABBIX

systemctl stop zabbix-server

-VER A SENHA DO MYSQL

egrep -v '^$|^#' /etc/zabbix/zabbix_server.conf

-ACESSAR O MYSQL PARA DESATIVAR log_bin_trust

mysql -uroot -p
password mysql> set global log_bin_trust_function_creators = 0; mysql> quit;

-INICIAR NOVAMENTE O SERVIÇO DO ZABBIX E VALIDAR ERROS NO LOG systemctl start zabbix-server && tail -f /var/log/zabbix/zabbix_server.log

PRONTO!

Procedimentos by Mvenzi em 19/04/24 01:37AM
