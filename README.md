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

Nesta etapa, a infraestrutura base foi criada na AWS para hospedar a aplicação. O processo seguiu as melhores práticas de segurança e organização.

1.  **Criação da Rede (VPC):**
    * [cite_start]Foi criada uma VPC para isolar os recursos do projeto. [cite: 1]
    * [cite_start]Dentro da VPC, foram configuradas **2 sub-redes públicas** (para acesso externo, como o servidor web) e **2 sub-redes privadas** (para recursos futuros, como bases de dados). [cite: 14]
    * [cite_start]Um **Internet Gateway** foi criado e conectado às sub-redes públicas para permitir a comunicação com a internet. [cite: 15]
  



---
2.  **Criação do Servidor (Instância EC2):**
    * [cite_start]Foi lançada uma instância EC2 do tipo `t2.micro` (dentro do Nível Gratuito) utilizando uma imagem **Ubuntu Server LTS**. [cite: 16]
    * [cite_start]A instância foi colocada numa das **sub-redes públicas** para ser acessível externamente. [cite: 17]
    * Foi criado e associado um **Par de Chaves (`.pem`)** para garantir o acesso seguro via SSH.






3.  **Configuração do Firewall (Security Group):**
    * Foi criado um Security Group para controlar o tráfego de entrada.
    * [cite_start]Foram adicionadas regras para permitir tráfego na **porta 80 (HTTP)**, para que o site seja visível, e na **porta 22 (SSH)**, para gestão remota do servidor. [cite: 17]



