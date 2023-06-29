# Monitoramento Tronco SIP do Asterisk

#voip #sip #asterisk

Este guia descreve o processo de monitoramento do tronco SIP no Asterisk usando o Zabbix Agent. Certifique-se de ter o Zabbix instalado e configurado antes de prosseguir. O monitoramento é realizado utilizando o comando asterisk -rx "sip show peer <NomeSIP>", filtrando a saída para buscar o campo "status" e verificar se o valor é igual a "ok". Caso o valor desejado não seja encontrado, uma trigger é disparada.

---

## User Parameterse

1. Crie um arquivo para os parâmetros do monitoramento:
  
  ```bash
  nano /etc/zabbix/zabbix_agent.d/userparameter_asterisk.conf
  ```
  
2. Insira os parâmetros:
  
  ```bash
  # Asterisk check trunck status
  UserParameter=trunkstatus[*],/usr/bin/sudo /usr/sbin/asterisk -rx "sip show peer $1" | sed -n 's/.*Status//p' | sed 's/ //g' | sed 's/://g' | awk -F"(" '{print $$1}'
  ```
  

---

## Permissões

1. Crie um arquivo do diretório `sudoers.d` para adicionar as permissões necessárias para o monitoramento.
  
  ```bash
  nano /etc/sudoers.d/zabbix
  ```
  
2. Adicione o seguinte parâmetro:
  
  ```bash
  #Monitoramento Asterisk
  zabbix ALL=NOPASSWD: /usr/sbin/asterisk,/bin/sed,/usr/bin/awk
  ```
  

---

Configuração da Template
Adicione a template ao Host que deseja monitorar.

1. Edite o macro SIP_NAME adicionando o nome do SIP que deseja monitorar.

2. Abra as configurações do host.

  1. Navegue até a aba "Macro".

  2. Clique em "Macros herdadas e do Host".

  3. Busque pelo macro {$SIP_NAME} e clique em "Modificar".

  4. Insira o nome do SIP no campo de valor do Macro (segundo campo).
    
  > **Note**
  > O valor declarado aqui deve ser igual à configuração definida no Asterisk. Caso contrário, o comando irá falhar e o monitoramento não será bem-sucedido.
    
3. Salve as alterações.

Certifique-se de revisar e testar as etapas descritas nesta documentação para garantir que tudo esteja correto e funcione conforme o esperado.
