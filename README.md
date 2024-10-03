
# Criando uma rede com 6 computadores 2 switches e 2 servidores

Vamos criar uma rede, configurando com dhcp para atribuição dinamica dos ip's, vamos configurar um servidor DNS e um servidor web

Vamos usar o Cisco Packet Tracer para simular o ambiente.

Primeiro passo é selecionar os componentes, vamos precisar de:

6 computadores \
2 switches, vamos usar o 2950-24 \
1 Roteador 1841 \
2 servidor \
1 cabos de rede 


1º passo:
    Vamos fazer a conexão física entre os PCs com o switch. Nesse caso, usamos o cabo de conexão direta. Agora teremos que conectar o roteador no switch. Para isso, também vamos utilizar um cabo de conexão direta.

2º Passo:
    Vamos acessar o CLI do roteador para ativa-lo, 

Ativar permissoes para configurar, rode o comando
```
  enable
```
Agora entrar no modo de configuração
```
configure terminal
```
Agora acessamos a interface fastEthenert 0/0 onde ligamos o cabo do switch
```
interface fastEthernet 0/0
```
Para ativar rodamos
```
no shutdown
```
- Observamos que a conexão entre o switch e o roteador ficou verde

Agora podemos configurar o DHCP, para isso precisamos sair do modo de configuração da interface, com o comando exit

Podemos usar o comando ip ? para ver todas as opções de configurações do ip, para configurar o DHCP rodamos o seguinte comando
```
ip dhcp pool nome_da_rede
```

agora estamos na configuração do dhcp, usando a ? temos as opções

```
Router(dhcp-config)#?
  default-router  Default routers
  dns-server      Set name server
  domain-name     Domain name
  exit            Exit from DHCP pool configuration mode
  network         Network number and mask
  no              Negate a command or set its defaults
  option          Raw DHCP options
```

Agora vamos configurar a network, vamos usar o ip 193.168.3.0 e a mascara 255.255.255.0

```
network 193.168.3.0 255.255.255.0
```

Agora que atribuímos o endereço para a rede que acabamos de configurar no modo DHCP, precisamos informar qual é o portão de saída dessa rede, isto é, o default-router (roteador padrão).

```
default-router 193.168.3.1
```
Nesse momento, temos todas as configurações necessárias para o funcionamento do DHCP na nossa rede. Agora resta atribuir o mesmo endereço inserido no default-router para a porta FastEthernet0/0, para isso saimos do modo de configuração do DHCP com o `exit` em seguida acessamos a `interface fastEthernet0/0`

vamos atribuir um endereço IP para essa interface. Então, digitamos o comando ip address seguido do endereço 193.168.3.1, exatamente o mesmo que inserimos como default-router. Lembre-se de informar também a máscara de rede, que será na classe C (255.255.255.0).

```
ip address 193.168.3.1 255.255.255.0
```

Assim finalizamos a configuração do roteador, agora precismos atribuir os ip de forma dinamica nos computadores, para isso é só alterar de `static` para `dhcp` nas configurações de redes de cada computador


## Configurando um servidor web

Para o Servidor web, vamos adicionar na rede mais um switch pensando na expansão da rede e vamos adicionar um servidor web também, apos adicionar e fazer as conexões, vamos configurar no roteador essa nova interface que adicionamos, conetamos na porta fastEthenert 0/1, vamos acessar o CLI do roteador para configurar

Seguindo os seguintes comandos
```
enable

configure terminal

interface fastEthenert 0/1
```

Com isso, habilitamos a permissao, entramos no modo de configuracao e entramos na configuracao da porta 0/1 

Agora ativamos a porta

```
no shutdown
```

Em seguida atribuimos um ip de forma estática, Para isso, digitamos o comando ip address seguido do endereço, que pode ser 9.9.9.1. Também precisamos inserir a máscara de sub-rede. Conforme visto anteriormente, a máscara da classe A tem 255 no primeiro octeto e 0 nos demais.
```
ip address 9.9.9.1 255.0.0.0
```

Agora vamos acessar o servidor para configurar o ip dele. Vamos utilizar a configuração estática (Static). Como endereço de IP, podemos usar, por exemplo, `9.0.255.255`. Parece estranho, mas esse endereço é normal e plenamente atribuível, e nesse caso, usamos o endereço de IP da rede na qual estamos conectando na porta do roteador. A máscara de IP será preenchida automaticamente ao teclar "Enter" (255.0.0.0).

Na opção "Default Gateway", começaremos configurando a atribuição estática do endereço IP. Para isso, precisamos inserir o IP da porta FastEthernet0/1 do roteador, que é 9.9.9.1.


## Configurando um servidor DNS
