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
# Exemplo de navegação para a pasta Downloads
cd Downloads





