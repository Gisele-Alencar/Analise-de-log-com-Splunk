# Análise de Logs SSH com Splunk

![Splunk](https://img.shields.io/badge/splunk-%23000000.svg?style=for-the-badge&logo=splunk&logoColor=white)
![SPL](https://img.shields.io/badge/SPL_(Search_Processing_Language)-%23000000.svg?style=for-the-badge&logo=splunk&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

> Este laboratório prático foi desenvolvido com base no projeto oficial do [Haxcamp](https://app.haxcamp.com/projects/353bc3c9-c9e9-495e-8472-cc80fa5539f6/resources).

> A resolução teve como foco a aplicação de competências reais de Blue Team e Operações de Segurança (SOC), consolidando técnicas de Threat Hunting e investigação de incidentes.

## Visão Geral 

* **Logins bem-sucedidos:** Identificar quem se ligou e a partir de onde.
* **Tentativas de login falhadas:** Possível *password spraying* ou força bruta.
* **Múltiplas tentativas de autenticação com falha:** Indicadores claros de ataques de força bruta.
* **Ligações sem autenticação:** Possível *port scanning* ou sessões incompletas.

## Laboratório e Preparação
Para a execução, foi utilizada uma instância do Splunk (Enterprise/Free) e um ficheiro de logs em formato JSON (`ssh_log.json`).

1. Ingestão de dados via *Apps > Search & Reporting > Add Data > Upload*.
2. Carregamento do ficheiro `ssh_log.json`.
3. Definição do `sourcetype = _json` para extração automática de campos.
4. Criação e indexação num novo índice chamado `ssh_logs`.

---

## Consultas SPL

## Análise de Tentativas de Login Falhadas
Identificação dos 10 principais endereços IP de origem responsáveis por gerar falhas de login.

<img width="1356" height="648" alt="spul1" src="https://github.com/user-attachments/assets/3291ce3c-7625-4e47-b7bc-8c96f6fa4e57" />


## Deteção de Ataque de Força Bruta
Pesquisa por múltiplas tentativas falhadas (ex: mais de 5 tentativas) para configurar um alerta no Splunk (acionado quando um IP tenta mais de 5 logins em 10 minutos).

<img width="1337" height="380" alt="spul5" src="https://github.com/user-attachments/assets/69b8fb9c-9cf2-4ca8-ac1f-43bd6faa9898" />


## Rastreamento de Logins Bem-sucedidos
Monitorização de acessos validados para posterior cruzamento com falhas anteriores (deteção de contas comprometidas).

<img width="1355" height="612" alt="spul4" src="https://github.com/user-attachments/assets/97969308-779a-4b3b-b8f4-7faadc758797" />

## Configuração de Alerta (Força Bruta)
Foi configurado um **Alerta no Splunk** para monitorizar e sinalizar este comportamento de multiplas tentativas.

<img width="1360" height="597" alt="sql3" src="https://github.com/user-attachments/assets/8a2097ae-18b6-43cb-a1be-32ba9a378075" />

---

## Conclusão
Nessa investigação prática, foi possível atingir os seguintes resultados:
* Criação de *dashboards* para a monitorização contínua de atividades SSH.
* Identificação com sucesso de tentativas de login por força bruta e acessos suspeitos à infraestrutura.
* Configuração e validação de alertas automatizados no Splunk para sinalizar comportamentos de alto risco.
* Desenvolvimento prático de competências na pesquisa (SPL), análise e visualização de dados críticos de segurança.

---
<p align="center">@2026 Gisele Alencar</p>
