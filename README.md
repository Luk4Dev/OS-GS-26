# Rede Frio — Operating Systems | FIAP Global Solution 2026.2

**Turma:** 3ESPA  
**Equipe:** Diego Garcia Tosta — RM 556724; Joud Jihad Jaber — RM 556482; Lucca Pereira — RM 560731.

## Problema e proposta
A Rede Frio é um protótipo CitizenTech de apoio à redução do desperdício alimentar. Conecta empresas doadoras a instituições receptoras, organiza doações e propõe monitoramento de refrigeradores. O site demonstrativo hospedado nesta atividade apresenta uma previsão **simulada** de 31,8 kg de alimentos excedentes, sem afirmar que os alimentos estejam seguros para doação.

## Ambiente demonstrado
- **Virtualização:** Oracle VirtualBox, modo de rede NAT.
- **Sistema:** Windows Server 2025 Standard Evaluation, com Desktop Experience.
- **Hostname:** `REDE-FRIO`.
- **IPv4 estático:** `10.0.2.15/24`, gateway `10.0.2.2`.
- **Domínio Active Directory:** `redefrio.test` — floresta de laboratório, domínio `REDEFRIO`.
- **Serviços instalados:** AD DS, DNS Server e IIS.
- **DNS:** resolução de `rede-frio.redefrio.test` para `10.0.2.15` demonstrada com `nslookup`; ocorreu um aviso de timeout na resposta inicial, mas a resolução foi retornada.
- **IIS:** Default Web Site, HTTP porta 80, diretório padrão `C:\inetpub\wwwroot`, com site estático Rede Frio (`site/index.html`).
- **NTFS:** evidenciado acesso de leitura/execução para o grupo `Users` na pasta wwwroot; não se demonstra uma política personalizada completa de menor privilégio.
- **Monitoramento:** PerfMon com contadores de CPU, memória e espaço livre no disco; conjunto `RedeFrio-Alertas` demonstrado em estado Running.
- **Auditoria:** Event Viewer (Application) e logs W3C do IIS com acessos HTTP 200.
- **Falhas:** teste CONTROLADO de parada e retomada **manual** do site no IIS, com capturas do erro e do retorno da página.
- **Recuperação:** opções de reinício do serviço W3SVC configuradas; Rapid-Fail Protection do pool habilitada. Não foi demonstrado teste de recuperação **automática** do serviço/pool.
- **Conectividade:** `Test-NetConnection localhost -Port 80` com `TcpTestSucceeded: True` (teste local na VM; não comprova acesso de outra máquina).

## Comandos executados na VM e evidenciados

```powershell
ipconfig /all
nslookup rede-frio.redefrio.test 10.0.2.15
Test-NetConnection localhost -Port 80
```

## Como reproduzir o site no IIS
1. Em VM Windows Server, instale a função **Web Server (IIS)**.
2. Coloque o arquivo `site/index.html` no diretório `C:\inetpub\wwwroot` (mantendo as permissões adequadas).
3. No IIS Manager, confirme **Default Web Site → Started** na porta 80.
4. Acesse `http://localhost` e execute `Test-NetConnection localhost -Port 80`.

**Nota de fidelidade:** o site publicado é uma **página demonstrativa estática**. Não é a API Java nem o aplicativo Mobile completo. A captura mostra uma previsão *simulada*, não dados de operação real.

## Evidências
A pasta `evidencias/` contém **21 capturas reais** da máquina virtual, numeradas sequencialmente. Veja o índice em `docs/INDICE_EVIDENCIAS.md`.

## Pontos ainda não integralmente demonstrados
- Não há captura da GPO criada/aplicada nesta instalação.
- O conjunto de alertas está em `Running`, mas ainda é necessário conferir documentalmente os três limites e, para demonstrar disparo, testar as condições controladas.
- Configuração de recuperação do W3SVC e proteção do pool não equivalem a comprovar recuperação automática durante uma falha real.
- A publicação demonstrada é uma página estática mínima, não uma aplicação web completa integrada.

Essas limitações são registradas para não confundir configuração com execução comprovada.

## Vídeo
Gravar vídeo de demonstração de **até 7 minutos** mostrando a VM e as evidências conforme `docs/ROTEIRO_VIDEO.md`. Entrega via link do GitHub no Teams, conforme o enunciado.
