# Relatório de Investigação de Segurança

**Projeto:** SOC Lab — Investigação de Brute Force via SSH

**Identificador:** SOC-SSH-001

**Data dos eventos:** 07/10/2026

**Ambiente:** Laboratório virtualizado com Ubuntu e Kali Linux

**Classificação final:** Simulação autorizada — sem comprometimento confirmado

---

## 9.1 — Resumo executivo

Durante uma simulação controlada de segurança, foram identificadas três falhas de autenticação SSH contra o usuário `samuel`, originadas do endereço IP `192.168.64.10`.

As tentativas ocorreram em um intervalo de 22 segundos, atendendo aos critérios da regra conceitual SOC-SSH-001.

A análise dos registros também identificou uma autenticação bem-sucedida posterior, realizada em outra conexão SSH.

A atividade foi conduzida em ambiente autorizado, sem evidências de comprometimento real.

## 9.2 — Descrição do incidente

O laboratório foi composto por duas máquinas virtuais:

- **SOC-KALI:** 192.168.64.10 — origem das tentativas.
- **SOC-SERVER:** 192.168.64.9 — servidor Ubuntu com OpenSSH.

Foram realizadas tentativas de autenticação SSH com senhas incorretas contra o usuário `samuel`.

O servidor registrou mensagens `Failed password`, permitindo a análise dos eventos e a avaliação de um possível comportamento de força bruta.

Trata-se de uma investigação simulada, não de um incidente real confirmado.

## 9.3 — Timeline

| Data e horário | Evento |
| --- | --- |
| 07/10/2026 14:03:38 | Primeira falha de autenticação SSH |
| 07/10/2026 14:03:57 | Segunda falha de autenticação SSH |
| 07/10/2026 14:04:00 | Terceira falha de autenticação SSH |
| 07/10/2026 14:04:01 | Conexão encerrada antes de autenticação bem-sucedida |
| 07/10/2026 14:20:03 | Autenticação SSH posterior bem-sucedida |

## 9.4 — Evidências

As evidências foram obtidas por meio dos registros do OpenSSH no Ubuntu, principalmente utilizando `journalctl`.

Foram analisadas:

- Mensagens de falha de autenticação.
- Endereço IP e usuário relacionados aos eventos.
- Horários das tentativas.
- Encerramento da conexão.
- Registro de autenticação bem-sucedida posterior.
- Validação manual da condição SOC-SSH-001.

**Evidência principal da detecção:** `06-01-validacao-regra-ssh.png`.

Os demais prints permanecem organizados nas respectivas pastas do projeto.

## 9.5 — IOCs e indicadores observados

**IP de origem:** 192.168.64.10

**IP de destino:** 192.168.64.9

**Conta investigada:** samuel

**Serviço:** SSH / TCP 22

**Evento:** Failed password

**Padrão:** três falhas em 22 segundos.

Os indicadores foram observados durante uma simulação autorizada. O IP de origem não foi classificado como malicioso.

Não foram identificados indicadores que comprovem comprometimento real.

## 9.6 — Análise

Ao analisar os logs do SSH, identifiquei três falhas de autenticação para o usuário `samuel`, vindas do IP `192.168.64.10`, em um intervalo de 22 segundos.

Essas tentativas atenderam aos critérios da regra SOC-SSH-001, definida para identificar três ou mais falhas do mesmo IP, contra o mesmo usuário, dentro de 60 segundos. A validação foi feita manualmente, usando comandos Linux, sem um SIEM.

Também identifiquei um login bem-sucedido às 14:20:03, vindo do mesmo IP, mas em outro processo SSH. Por isso, não posso afirmar que as tentativas anteriores resultaram nesse acesso.

Como o IP pertence ao Kali utilizado no laboratório, sabemos pelo contexto dos testes que a atividade foi autorizada. Porém, somente os logs não comprovam essa autorização.

Em uma empresa, seria necessário confirmar quem realizou o login e verificar se houve alguma atividade suspeita depois dele. Essa verificação adicional não foi realizada neste laboratório.

**Conclusão:** o comportamento é compatível com tentativas de adivinhar uma senha, mas não encontramos evidências de comprometimento real nos registros analisados.

## 9.7 — MITRE ATT&CK

**Tática:** Credential Access — TA0006

**Técnica:** Brute Force — T1110

**Subtécnica:** Password Guessing — T1110.001

As tentativas de autenticação com senhas incorretas foram relacionadas à técnica Password Guessing.

O mapeamento representa o comportamento simulado, sem atribuição de atividade maliciosa real.

Referência: [https://attack.mitre.org/techniques/T1110/001/](https://attack.mitre.org/techniques/T1110/001/)

## 9.8 — Ações realizadas

1. Preparação do laboratório com Ubuntu e Kali Linux.
2. Configuração e verificação do serviço SSH.
3. Geração controlada de tentativas de autenticação.
4. Coleta e análise de registros do OpenSSH.
5. Identificação do IP de origem, usuário e horários.
6. Correlação das três falhas de autenticação.
7. Validação manual da regra SOC-SSH-001.
8. Análise de autenticação bem-sucedida posterior.
9. Avaliação da necessidade de contenção.
10. Documentação das evidências e conclusões.

Nenhum bloqueio de IP ou isolamento foi necessário.

## 9.9 — Recomendações

Ao verificar as configurações do SSH, identifiquei que o servidor permite autenticação por senha (`passwordauthentication yes`) e por chave SSH (`pubkeyauthentication yes`). O login de root por senha está impedido (`permitrootlogin prohibit-password`).

Como simulamos tentativas de senha incorreta, percebi que permitir esse tipo de acesso pode facilitar novas tentativas. Isso não significa, por si só, que o servidor tenha uma vulnerabilidade.

Uma melhoria seria usar chaves SSH e, após confirmar que o acesso por chave funciona, avaliar a desativação do acesso por senha. Também seria importante restringir quem pode acessar o serviço e acompanhar os registros de autenticação.

Essas são recomendações. Não alterei a configuração do servidor durante essa etapa.

## 9.10 — Conclusão

A investigação identificou três falhas de autenticação SSH em um intervalo de 22 segundos, atendendo aos critérios definidos pela regra SOC-SSH-001.

A análise demonstrou como registros de autenticação podem ser utilizados para identificar padrões suspeitos, correlacionar eventos e avaliar a necessidade de resposta.

Por se tratar de uma simulação autorizada, não houve comprometimento real confirmado nem necessidade de contenção.

O projeto permitiu exercitar competências relacionadas à análise de logs, investigação de alertas, validação manual de regras, interpretação de evidências, MITRE ATT&CK e documentação técnica de segurança.

## Análise complementar — Uso de IA na investigação

### Objetivo

Verificar se a inteligência artificial poderia ajudar a interpretar os logs e organizar as informações da investigação.

### Como foi feita

Enviei ao ChatGPT seis registros selecionados do OpenSSH, incluindo as falhas de autenticação, o encerramento da conexão e o login bem-sucedido posterior.

A IA organizou os horários, identificou o IP e o usuário, e destacou que o login posterior ocorreu em outro processo SSH.

### Comparação com a investigação manual

Comparei a resposta da IA com os registros que já havia analisado. As informações principais estavam corretas: três falhas em 22 segundos, mesmo IP e usuário, e um login posterior em outra conexão.

O uso da IA ajudou a organizar a análise, mas ela recebeu apenas os seis registros selecionados. Por isso, não substitui a conferência dos logs nem comprova que todo o ambiente foi investigado.

### Conclusão

A IA foi útil para revisar e organizar os resultados. A interpretação final continuou dependendo da análise dos registros e do contexto conhecido do laboratório.
