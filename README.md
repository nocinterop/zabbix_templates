# zabbix_templates
Versão 6.0
Template versão Zabbix 6.0 para O.S. LINUX customizado :
Para o funcionamento da descoberta de serviços utilizar os comandos abaixo dentro de um arquivo .conf em /etc/zabbix/zabbix_agentd.d/ no host que queremos monitorar.

OBS1:. Importante validar se dentro do arquivo de configuração do agente Zabbix, o campo Include= está com o mesmo 'path' (/etc/zabbix/zabbix_agentd.d/) onde se encontra o arquivo .conf

OBS2:. Caso seja necessário remover serviços da descoberta, incluir o nome do serviço no filtro do comando egrep -v "@|nome_do_serviço" dos comando do UserParameter


- Rocky Linux:

UserParameter=services.raw,echo "{\"data\":[$(systemctl list-unit-files|grep "\.service"|egrep -v "@|static|alias|indirect|disable         disabled|tuned|systemd|Network|rhel|mcollective|rhsmcertd|puppet|dbus-org|lvm2-|chronyd|auditd|kdump|goferd|microcode|irqbalance|selinux-autorelabel|blk-availability|rpmdb-"|sed -E -e "s/\.service\s+/\",\"status\":\"/;s/(\s+)?$/\"},/;s/^/{\"name\":\"/;$ s/.$//")]}" | python -m json.tool

UserParameter=systemctl.status[*],state=$(systemctl status zabbix-agent | grep Active | cut -d ' ' -f 7); test $state = 'active' && echo "1" || echo "0"

- Other Linux:

UserParameter=services.raw,echo "{\"data\":[$(systemctl list-unit-files|grep "\.service"|egrep -v "@|static|alias|indirect|disable         disabled|tuned|systemd|Network|rhel|mcollective|rhsmcertd|puppet|dbus-org|lvm2-|chronyd|auditd|kdump|goferd|microcode|irqbalance|selinux-autorelabel|blk-availability|rpmdb-"|sed -E -e "s/\.service\s+/\",\"status\":\"/;s/(\s+)?$/\"},/;s/^/{\"name\":\"/;$ s/.$//")]}" | python -m json.tool

UserParameter=systemctl.status[*],state=$(systemctl status zabbix-agent | grep Active | cut -d ' ' -f 5); test $state = 'active' && echo "1" || echo "0"

- Arquivo do Template YAML

Template Linux INTEROP Custom


Template versão Zabbix 6.0 para O.S. Windows customizado :
Obs:

Sobre a descoberta dos serviços Windows, o mesmo realiza a descoberta apenas de serviços que estão com sua configuração para iniciar automaticamente com o S.O e também esta com filtro para não descobrir certos tipos de serviços, caso tenha a necessidade de adicionar serviços a esta lista de exclusão precisa ir na MACRO do template e adicionar o name do serviço, segue abaixo:

{$SERVICE.NAME.NOT}


MSSQL by ODBC
Overview
For Zabbix version: 6.2 and higher
The template is developed for monitoring DBMS Microsoft SQL Server via ODBC.

This template was tested on:

Microsoft SQL, version 2017, 2019
Setup
See Zabbix template operation for basic instructions.

Create an MSSQL user for monitoring. For example, zbx_monitor. View Server State and View Any Definition permissions should be granted to the user. Grant this user read permissions to the sysjobschedules, sysjobhistory, sysjobs tables. For example, using T-SQL commands: GRANT SELECT ON OBJECT::msdb.dbo.sysjobs TO zbx_monitor GRANT SELECT ON OBJECT::msdb.dbo.sysjobservers TO zbx_monitor GRANT SELECT ON OBJECT::msdb.dbo.sysjobactivity TO zbx_monitor GRANT EXECUTE ON OBJECT::msdb.dbo.agent_datetime TO zbx_monitor For more information, see MSSQL documentation: Create a database user GRANT Server Permissions Configure a User to Create and Manage SQL Server Agent Jobs
Set the username and password in host macros ({$MSSQL.USER} and {$MSSQL.PASSWORD}). Do not forget to install Microsoft ODBC driver on Zabbix server or Zabbix proxy. See Microsoft documentation for instructions: https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server?view=sql-server-ver15. Note! Credentials in the odbc.ini do not work for MSSQL.
For named instance set the value of {$MSSQL.INSTANCE} macro as MSSQL$instance name. In case if MSSQL was installed using default configuration do not change {$MSSQL.INSTANCE} macro value.

The "Service's TCP port state" item uses {HOST.CONN} and {$MSSQL.PORT} macros to check the availability of MSSQL instance. If your instance uses a non-default TCP port, set the port in your section of odbc.ini in the line Server = IP or FQDN name, port.

Zabbix configuration
No specific Zabbix configuration is required.
