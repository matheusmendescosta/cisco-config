---

# Configuração de uma Rede com 6 Computadores, 2 Switches, 1 Roteador e 2 Servidores

Neste guia, vamos configurar uma rede utilizando DHCP para atribuição dinâmica de IPs, além de configurar um servidor DNS e um servidor web. A simulação será feita no **Cisco Packet Tracer**.

## Componentes Necessários

- 6 Computadores
- 2 Switches (Modelo: 2950-24)
- 1 Roteador (Modelo: 1841)
- 2 Servidores
- Cabos de Rede

## Passo 1: Conexão Física dos Dispositivos

1. Conecte os 6 computadores aos **switches** utilizando **cabos de conexão direta**.
2. Conecte o **roteador** ao **switch principal** também com um **cabo de conexão direta**.
3. Verifique as conexões físicas. Após realizar todas as conexões, estamos prontos para configurar os dispositivos.

## Passo 2: Configuração do Roteador

### Ativando o Roteador

1. Acesse o CLI do roteador e ative o modo de configuração com os seguintes comandos:
   ```bash
   enable
   configure terminal
   ```
   
2. Acesse a interface FastEthernet0/0, onde o cabo do switch está conectado:
   ```bash
   interface fastEthernet 0/0
   no shutdown
   ```
   - O LED da interface deve ficar verde, indicando que a conexão está ativa.

### Configurando o DHCP

1. Saia da interface para configurar o DHCP:
   ```bash
   exit
   ```
2. Crie um pool de endereços DHCP:
   ```bash
   ip dhcp pool nome_da_rede
   ```
3. Configure a rede com o IP 193.168.3.0 e máscara 255.255.255.0:
   ```bash
   network 193.168.3.0 255.255.255.0
   ```
4. Defina o roteador padrão (gateway) para a rede:
   ```bash
   default-router 193.168.3.1
   ```
5. Saia do modo DHCP e atribua o mesmo endereço IP ao roteador (interface FastEthernet0/0):
   ```bash
   exit
   interface fastEthernet 0/0
   ip address 193.168.3.1 255.255.255.0
   ```
   - Agora, o DHCP está configurado e os computadores podem receber endereços IP de forma dinâmica.

## Passo 3: Configuração dos Computadores

Nos **computadores**, altere as configurações de rede para **DHCP**, permitindo que o servidor DHCP configure os IPs automaticamente.

---

## Configurando um Servidor Web

1. Adicione um novo **switch** à rede para conectar o servidor web, permitindo a expansão futura.
2. Conecte o **servidor web** ao switch e configure a interface FastEthernet0/1 no roteador:
   ```bash
   enable
   configure terminal
   interface fastEthernet 0/1
   no shutdown
   ```
3. Atribua um endereço IP estático à interface FastEthernet0/1:
   ```bash
   ip address 9.9.9.1 255.0.0.0
   ```

### Configuração do Servidor Web

1. No servidor, defina o IP de forma estática. Por exemplo:
   - **Endereço IP:** `9.0.255.255`
   - **Máscara de Sub-rede:** 255.0.0.0 (preenchida automaticamente).
   - **Gateway Padrão:** `9.9.9.1` (IP da porta FastEthernet0/1 do roteador).

---

## Configurando um Servidor DNS
