# Análise de Logs SSH com Splunk

![Splunk](https://img.shields.io/badge/splunk-%23000000.svg?style=for-the-badge&logo=splunk&logoColor=white)
![SPL](https://img.shields.io/badge/SPL_(Search_Processing_Language)-%23000000.svg?style=for-the-badge&logo=splunk&logoColor=white)
![Redes](https://img.shields.io/badge/Redes_%26_Protocolos-00599C?style=for-the-badge)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

## Visão Geral 
Analise de logs SSH para detectar autentificação:
* **Logins bem-sucedidos:** Identificar quem se ligou e a partir de onde.
* **Tentativas de login falhadas:** Possível *password spraying* ou força bruta.
* **Múltiplas tentativas de autenticação com falha:** Indicadores claros de ataques de força bruta.
* **Ligações sem autenticação:** Possível *port scanning* ou sessões incompletas.

## ⚙️ Configuração do Laboratório e Preparação
Para a execução deste laboratório, foi utilizada uma instância do Splunk (Enterprise/Free) e um ficheiro de logs em formato JSON (`ssh_log.json`).

**Passos de Preparação:**
1. Ingestão de dados via *Apps > Search & Reporting > Add Data > Upload*.
2. Carregamento do ficheiro `ssh_log.json`.
3. Definição do `sourcetype = _json` para extração automática de campos.
4. Criação e indexação num novo índice chamado `ssh_logs`.

---

## Consultas SPL

## Ingestão e Validação de Logs

<img width="1349" height="609" alt="spul2" src="https://github.com/user-attachments/assets/06469b24-639b-4f17-b5de-3aa6e581ea0b" />


## Análise de Tentativas de Login Falhadas
Identificação dos 10 principais endereços IP de origem responsáveis por gerar falhas de login.

<img width="1356" height="648" alt="spul1" src="https://github.com/user-attachments/assets/3291ce3c-7625-4e47-b7bc-8c96f6fa4e57" />


Tarefa 3: Deteção de Ataque de Força Bruta
Pesquisa por múltiplas tentativas falhadas (ex: mais de 5 tentativas) para configurar um alerta no Splunk (acionado quando um IP tenta mais de 5 logins em 10 minutos).

Consulta SPL:

Splunk SPL
index=ssh_logs event_type="Multiple failed authentication attempts" | stats count by id.orig_h, id.resp_h
📸 IMAGEM DA TAREFA 3:
[ARRASTE A FOTO DO ALERTA CONFIGURADO OU DA LISTA DE MÚLTIPLAS FALHAS AQUI]

Tarefa 4: Rastreamento de Logins Bem-sucedidos
Monitorização de acessos validados para posterior cruzamento com falhas anteriores (deteção de contas comprometidas).

Consulta SPL:

Splunk SPL
index=ssh_logs event_type="Successful SSH login" | stats count by id.orig_h, id.resp_h
📸 IMAGEM DA TAREFA 4:
[ARRASTE A FOTO DO DASHBOARD DE LOGINS BEM-SUCEDIDOS AQUI]

Tarefa 5: Identificação de Ligações Suspeitas (Sem Autenticação)
Análise de ligações SSH não autenticadas ao longo do tempo para detetar potenciais varreduras de portas (port scanning) ou sondagem SSH.

Consulta SPL (Timechart):

Splunk SPL
index=ssh_logs event_type="Connection without authentication" | timechart count by id.orig_h
📸 IMAGEM DA TAREFA 5:
[ARRASTE A FOTO DO GRÁFICO DE TEMPO (TIMECHART) AQUI]

🎯 Conclusão
Ao concluir este projeto de investigação, foi possível:

Criar dashboards para monitorizar continuamente a atividade SSH.

Identificar proativamente tentativas de login por força bruta e acessos suspeitos.

Configurar alertas automatizados no Splunk para comportamentos de alto risco.

Aplicar na prática competências fundamentais de um SOC Analyst, tais como análise de logs, criação de consultas SPL e visualização de dados de segurança.

### Tarefa 1: Ingestão e Validação de Logs
Validação da correta extração dos campos essenciais (`event_type`, `auth_success`, `auth_attempts`, `id.orig_h`, `id.resp_h`).

**Consulta de Validação:**
```spl
index=ssh_logs | stats count by event_type
