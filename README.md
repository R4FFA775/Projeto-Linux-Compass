# Projeto-Linux-Compass
Projeto de configuração de servidor web com monitoramento e alertas automatizados

## Objetivo do Projeto
[cite_start]O objetivo deste projeto foi desenvolver e testar habilidades em Linux, AWS e automação através da configuração de um ambiente de servidor web Nginx monitorado, que envia alertas em caso de falha. [cite: 13, 1]

## Ferramentas e Tecnologias Utilizadas
* **Cloud:** AWS (EC2, VPC, Security Groups)
* **Sistema Operacional:** Linux (Ubuntu Server 24.04 LTS)
* **Servidor Web:** Nginx
* **Linguagem de Script:** Bash Scripting
* **Automação:** Cron (Agendador de Tarefas)
* **Alertas:** Discord Webhooks

---

## Etapas de Implementação

### Etapa 1: Configuração do Ambiente na AWS

#### Guia de Criação Detalhado

O processo de criação da infraestrutura no console da AWS seguiu os seguintes passos: A chave que vc criou

**1. Criação da Rede (VPC):**

* No console da AWS, foi acedido o serviço **VPC**.
* Foi selecionada a opção **"Criar VPC"** e, em seguida, o assistente **"VPC e mais"** para automatizar a criação dos componentes.
* As configurações aplicadas foram:
    * **Nome:** `projeto-final-vpc`
    * **Zonas de Disponibilidade (AZs):** `2`
    * **Número de sub-redes públicas:** `2`
    * **Número de sub-redes privadas:** `2`
    * **Gateways NAT:** `Nenhum`
    * **Endpoints da VPC:** `Gateway do S3`
* Após a confirmação, o assistente criou a VPC, as 4 sub-redes, um **Internet Gateway** e as tabelas de rotas necessárias.

**2. Criação do Servidor (Instância EC2):**

* No console da AWS, foi acedido o serviço **EC2**.
* Foi selecionada a opção **"Executar instância"**.
* As configurações aplicadas foram:
    * **Nome:** `servidor-final-aws`
    * **AMI (Sistema Operacional):** `Ubuntu Server LTS` 
    * **Tipo de Instância:** `t2.micro` 
    * **Par de Chaves (Login):** Foi criado um novo par de chaves do tipo `RSA` e formato `.pem`, que foi descarregado e guardado em segurança para permitir o acesso SSH.
    * **Configurações de Rede:** A instância foi associada à **VPC** `projeto-final-vpc` e colocada numa das **sub-redes públicas**. A opção para atribuir um IP público automaticamente foi ativada.
    * **Firewall (Grupo de Segurança):** Foi criado um novo Security Group com as seguintes regras de entrada:
        * Permitir tráfego **SSH** (porta 22) para acesso administrativo.
        * Permitir tráfego **HTTP** (porta 80) para acesso ao servidor web.

* Finalmente, a instância foi executada clicando em **"Executar instância"**.

#### Acesso Remoto Seguro com SSH

Após a criação bem-sucedida da instância EC2, o passo seguinte foi estabelecer uma conexão remota segura para poder instalar e configurar o servidor web. Para isso, foi utilizado o protocolo **SSH (Secure Shell)**.

O acesso foi realizado a partir de um terminal local (Windows PowerShell), seguindo estes passos:

**1. Pré-requisitos:**
* A instância EC2 estava no estado "Em execução" (Running).
* O **Endereço IPv4 Público** da instância foi copiado do painel da AWS.
* O ficheiro da chave privada (**`.pem`**), descarregado durante a criação do Par de Chaves, estava guardado num local seguro no computador local.

**2. Navegação para a Pasta da Chave:**
No terminal local, foi utilizado o comando `cd` para navegar até à pasta onde o ficheiro `.pem` estava guardado.
```bash
cd Downloads
````

**3. Execução do Comando de Conexão:**

A conexão foi estabelecida com o seguinte comando(exemplo), que utiliza a chave privada para a autenticação em vez de uma senha:

```bash
# Estrutura: ssh -i "ficheiro-da-chave.pem" utilizador@ip-publico
ssh -i "suachave.pem" ubuntu@SEU_IP_PUBLICO_DA_AWS
```

Os componentes deste comando são:

  * **`ssh`**: O programa cliente que inicia a conexão.
  * **`-i "suachave.pem"`**: A flag `-i` (de identidade) que aponta para o ficheiro de chave privada a ser usado na autenticação.
  * **`ubuntu`**: É o nome de utilizador padrão para as imagens oficiais do Ubuntu Server na AWS.
  * **`@SEU_IP_PUBLICO_DA_AWS`**: O endereço público do servidor na nuvem.

**4. Confirmação de Autenticidade do Host:**

Na primeira vez que se conecta a um novo servidor, o cliente SSH exibe a "impressão digital" (fingerprint) do servidor e pede uma confirmação de confiança. Foi digitado `yes` para aprovar a conexão e adicionar o servidor à lista de hosts conhecidos. Esta é uma medida de segurança importante contra ataques "man-in-the-middle".

*Exemplo da confirmação de autenticidade:*
`[Coloque aqui o print do PowerShell a pedir a confirmação "yes/no"]`

Após estes passos, a conexão segura foi estabelecida com sucesso, permitindo a gestão completa do servidor remoto através da linha de comando, como se pode ver no resultado final.

*Exemplo da conexão bem-sucedida:*
`[Coloque aqui o print do seu terminal já conectado à instância AWS]`

```
```
### Etapa 2: Configuração do Servidor Web

Com o acesso SSH estabelecido, o passo seguinte foi a instalação e configuração do servidor web Nginx para exibir uma página HTML personalizada.

**1. Instalação do Nginx:**
O servidor foi primeiramente atualizado para garantir que todos os pacotes estivessem na sua versão mais recente, uma boa prática de segurança. Em seguida, o Nginx foi instalado utilizando o gestor de pacotes `apt`.

```bash
# Atualiza a lista de pacotes do sistema
sudo apt update

# Instala as atualizações pendentes
sudo apt upgrade -y

# Instala o Nginx
sudo apt install nginx -y
```
* Após a instalação, foi verificado o estado do serviço para garantir que ele estava a funcionar. O serviço Nginx geralmente inicia automaticamente, mas é uma boa prática confirmar.
    ```bash
    # Verifica o estado atual do Nginx
    sudo systemctl status nginx
    ```
* Para garantir que o serviço Nginx inicie sempre que o servidor for ligado, o seguinte comando foi executado para o habilitar:
    ```bash
    # Habilita o Nginx para iniciar no boot
    sudo systemctl enable nginx
    ```

**2. Criação da Página Personalizada:**

* A página padrão do Nginx foi substituída por um ficheiro `index.html` personalizado, criado em `/var/www/html/`.
* O comando `sudo nano /var/www/html/index.html` foi usado para criar e editar o ficheiro com o seguinte conteúdo:

    ```html
    <!DOCTYPE html>
    <html lang="pt-br">
    <head>
        <title>Projeto DevSecOps</title>
        <style>
            body { font-family: sans-serif; text-align: center; margin-top: 50px; }
        </style>
    </head>
    <body>
        <h1>Meu Projeto Linux - DevSecOps na AWS!</h1>
        <p>Servidor Nginx configurado com sucesso por Razzur!</p>
    </body>
    </html>
    ```

**3. Verificação Final:**

* Com o serviço Nginx ativo, o acesso ao Endereço IPv4 Público da instância EC2 através de um navegador exibiu a página personalizada com sucesso.





