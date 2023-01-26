# Monitoramento Tronco SIP do Asterisk

#voip #sip #asterisk

Para o monitoramento do tronco SIP do Asterisk, e necessário já possuir o Zabbix Instalado e Configurado. O modo de monitoramento consiste em usar o comando `asterisk -rx "sip show peer <NomeSIP>"` , filtrar sua saída buscando o campo **status** e verificando se o valor é igual a **ok**, caso o valor desejado não for encontrado, a trigger e disparada.

---

## User Parameterse

1. Crie um arquivo para os parâmetros:
  
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

1. Crie um arquivo na pasta `sudoers.d`
  
  ```bash
  nano /etc/sudoers.d/zabbix
  ```
  
2. Adicione o seguinte parâmetro
  
  ```bash
  #Monitoramento Asterisk
  zabbix ALL=NOPASSWD: /usr/sbin/asterisk,/bin/sed,/usr/bin/awk
  ```
  

---

## Configurando a Template

1. Adicione a template no Host que deseja monitorar.
  
2. Edite o macro **SIP_NAME** adicionando o nome do SIP que deseja monitorar.
  
  1. Abra as configurações do host.
    
  2. Navegue até a aba macro
    
  3. Clique em **Macros herdadas e do Host**
    
  4. Busque pelo macro **{$SIP_NAME}** e clique em modificar.
    
  5. Insira o nome do SIP no campo do valor da do Macro (Segundo Campo)
    
    > **Note**
    > O valor declarado aqui deve ser **igual ao configurado no Asterisk**, caso contraio comando irá falhar e o monitoramento também.
    
3. Salve as alterações.
