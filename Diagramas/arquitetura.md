# Arquitetura do laboratório SOC

## Ambiente

O laboratório foi montado no UTM, utilizando duas máquinas virtuais:

- **SOC-KALI:** Kali Linux, IP `192.168.64.10`. Máquina utilizada para gerar as tentativas de autenticação.
- **SOC-SERVER:** Ubuntu Linux, IP `192.168.64.9`. Máquina que recebeu as conexões SSH e registrou os eventos.

## Diagrama

```text
+--------------------------+          SSH (TCP 22)         +--------------------------+
|         SOC-KALI         | ---------------------------> |        SOC-SERVER        |
|      192.168.64.10       |                              |       192.168.64.9       |
| Origem dos testes de SSH |                              | OpenSSH e logs do journal|
+--------------------------+                              +--------------------------+

                    Máquinas virtuais no UTM (MacBook)
```

## Como o laboratório funcionou

1. O Kali realizou tentativas de login SSH no Ubuntu.
2. O OpenSSH registrou as falhas e os acessos bem-sucedidos.
3. Os logs foram consultados com `journalctl` e filtrados com `grep`.
4. Três falhas do mesmo IP para o mesmo usuário em 22 segundos foram identificadas.
5. A regra conceitual `SOC-SSH-001` foi validada **manualmente**. Não foi implementado um SIEM ou detector automático.

**Observação:** o diagrama mostra a comunicação estudada, sem afirmar que as VMs estavam isoladas de todas as outras redes.
