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
