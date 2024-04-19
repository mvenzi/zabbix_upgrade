Zabbix upgrade 6.4 to 7.0<BR>

CRIAR AS PASTAS PARA O BACKUP<BR>

mkdir -p /mnt/bkp_zabbix/etc_zabbix/zabbix_conf_files;<BR>
mkdir -p /mnt/bkp_zabbix/usr_sbin/zabbix_server_files;<BR>
mkdir -p /mnt/bkp_zabbix/usr_share_doc/zabbix_files;<BR>
mkdir -p /mnt/bkp_zabbix/etc_nginx/conf_files;<BR>
mkdir -p /mnt/bkp_zabbix/etc_nginx/conf_d/conf_files;<BR>
mkdir -p /mnt/bkp_zabbix/php_fpm_d/zabbix_conf_files;<BR>
mkdir -p /mnt/bkp_zabbix/zabbixbd/bdzabbix_files;<BR>

-COPIAR OS BACKUP´S<BR>

cp -rp /etc/zabbix//mnt/bkp_zabbix/etc_zabbix/zabbix_conf_files;<BR>
cp -rp /usr/sbin/zabbix/mnt/bkp_zabbix/usr_sbin/zabbix_server_files;<BR>
cp -rp /usr/share/doc/zabbix-*/mnt/bkp_zabbix/usr_share_doc/zabbix_files;<BR>
cp -rp /etc/nginx/nginx.conf/mnt/bkp_zabbix/etc_nginx/conf_files;<BR>
cp -rp /etc/nginx/conf.d/zabbix.conf//mnt/bkp_zabbix/etc_nginx/conf_d/conf_files;<BR>
cp -rp /etc/php-fpm.d/mnt/bkp_zabbix/php_fpm_d/zabbix_conf_files;<BR>

-PARAR O SERVIÇO DO ZABBIX<BR>

systemctl stop zabbix-server<BR>

-VER A SENHA DO MYSQL<BR>

egrep -v '^$|^#' /etc/zabbix/zabbix_server.conf<BR>

-ACESSAR O MYSQL E ATIVAR log_bin_trust PARA ALTERAR AS TABELAS DO BANCO DURANTE A ATUALIZAÇÃO.<BR>

mysql -uroot -p<BR>
password<BR>
mysql> set global log_bin_trust_function_creators = 1;<BR>
mysql> quit;<BR>

-VALIDAR VERSÃO LINUX<BR>
cat /etc/redhat-release<BR>

-VALIDAR VERSÃO ZABBIX INSTALADO<BR>
rpm -qa | grep zabbix<BR>

-ACESSAR A PAGINA DA ZABBIX PARA PEGAR O RELEASE DO S.O.<BR>
https://www.zabbix.com/download<BR>

-INSTALAR O NOVO RELEAsE (exemplo: rocky linux 9) <BR>
rpm -Uvh https://repo.zabbix.com/zabbix/6.5/rocky/9/x86_64/zabbix-release-6.5-2.el9.noarch.rpm <BR>
dnf clean all <BR>

-ATUALIZAR OS PACOTES DO ZABBIX PARA NOVA VERSÃO <BR>
dnf update -y <BR>


-ATUALIZAR O BANCO DE DADOS PARA A NOVA VERSÃO <BR>
systemctl start zabbix-server && tail -f /var/log/zabbix/zabbix_server.log

-PARAR O SERVIÇO DO ZABBIX <BR>

systemctl stop zabbix-server<BR>

-VER A SENHA DO MYSQL<BR>

egrep -v '^$|^#' /etc/zabbix/zabbix_server.conf<BR>

-ACESSAR O MYSQL PARA DESATIVAR log_bin_trust<BR>

mysql -uroot -p <BR>
password <BR>
mysql> set global log_bin_trust_function_creators = 0; <BR>
mysql> quit; <BR>

-INICIAR NOVAMENTE O SERVIÇO DO ZABBIX E VALIDAR ERROS NO LOG <BR>
systemctl start zabbix-server && tail -f /var/log/zabbix/zabbix_server.log <BR>

PRONTO! <BR>

Procedimentos by Mvenzi em 19/04/24 01:37AM <BR>
