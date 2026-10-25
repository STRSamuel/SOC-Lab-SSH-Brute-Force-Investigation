# SOC Lab — Investigação de Brute Force via SSH

## Sobre o projeto

Laboratório prático de cibersegurança desenvolvido para exercitar conhecimentos iniciais de SOC (Security Operations Center) e Blue Team.

O projeto simula tentativas de autenticação SSH com senhas incorretas e apresenta a investigação dos registros gerados em um ambiente controlado e autorizado.

## Objetivos

- Analisar registros de autenticação SSH.
- Identificar tentativas de acesso malsucedidas.
- Correlacionar endereços IP, usuários e horários.
- Validar manualmente uma regra de detecção.
- Relacionar o comportamento ao MITRE ATT&CK.
- Documentar a investigação e suas conclusões.

## Ambiente utilizado

| Máquina | Sistema | IP | Função |
| --- | --- | --- | --- |
| SOC-KALI | Kali Linux | 192.168.64.10 | Origem das tentativas |
| SOC-SERVER | Ubuntu Linux | 192.168.64.9 | Servidor SSH investigado |

**Virtualização:** UTM

**Ferramentas:** OpenSSH, journalctl, grep e comandos Linux.

## Resultados

Durante a simulação, foram identificadas três falhas de autenticação SSH contra o usuário `samuel` em um intervalo de 22 segundos.

Foi definida e validada manualmente a regra conceitual SOC-SSH-001, baseada em três ou mais falhas de autenticação do mesmo IP contra o mesmo usuário dentro de 60 segundos.

Também foi identificado um login bem-sucedido posterior, analisado como um evento separado.

**Conclusão:** atividade simulada e autorizada, sem evidências de comprometimento real.

## MITRE ATT&CK

- **Tática:** Credential Access — TA0006.
- **Técnica:** Brute Force — T1110.
- **Subtécnica:** Password Guessing — T1110.001.

Referência: [https://attack.mitre.org/techniques/T1110/001/](https://attack.mitre.org/techniques/T1110/001/)

## Análise assistida por IA

Foi realizada uma análise complementar dos logs com auxílio de inteligência artificial.

As conclusões foram comparadas com a investigação manual, demonstrando a importância da validação humana ao utilizar IA em atividades de segurança.

## Documentação

- [Relatório da investigação](Relatorios/Relatorio-Investigacao-SSH.md)
- [Logs SSH utilizados](Logs/eventos-ssh.txt)
- [Arquitetura do laboratório](Diagramas/arquitetura.md)

As evidências visuais estão organizadas na pasta `Prints/`.

## Observações

Este projeto foi desenvolvido exclusivamente para fins educacionais, em laboratório próprio e autorizado.

A validação da regra de detecção foi manual. Não foi implementado um sistema SIEM ou um mecanismo automático de resposta a incidentes.
