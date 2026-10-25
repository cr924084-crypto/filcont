# Changelog

Todas as alterações relevantes e marcos de cada fase da plataforma são documentados neste arquivo.

## [Fase 7.1 - Vigência Fiscal Estrita e Alertas Preventivos de Manutenção] - 24/10/2026

### Alterado & Corrigido
- **Salário Mínimo e Legislação Oficial:**
  - Corrigida a citação oficial do Salário Mínimo de 2026 (R$ 1.621,00) para `"Decreto nº 12.797, de 23/12/2025 (DOU 24/12/2025)"` em todo o ecossistema (motores, seed, docs e testes).
  - Para 2024 (R$ 1.412,00) e 2025 (R$ 1.518,00), fontes expressamente anotadas como `"decreto citado, número a confirmar no Planalto"` até auditoria do responsável.
- **Regra de Ouro da Vigência Fiscal Estrita:**
  - Eliminado qualquer fallback arbitrário ou silencioso nos motores de cálculo. Na ausência de vigência cadastrada para a competência/ano, o cálculo lança erro explícito com instrução clara.
- **Varredura e Migração Completa de Constantes Fixas (`docs/conferencia/constantes-fixas.md`):**
  - Mapeamento e migração de todas as constantes de Simples Nacional (Anexos I a V e Fator R 28%), Lucro Presumido (presunções 8%/16%/32% e alíquotas IRPJ/CSLL/PIS/COFINS), Lucro Real (trava de 30% e isenção de adicional), Calendário Fiscal (feriado da Consciência Negra pós-2024 e prazos dos dias 20 e 25), Retenções Reinf (11%, 3,5%, 1,5%, 4,65%) e Simulador Tributário (faixas e alíquotas teste 2026 LC 214/2025).

### Adicionado
- **Comando de Auditoria de Casos Pendentes (`npm run casos:pendentes`):**
  - Script executável `scripts/casos-pendentes.mjs` que lista casos não homologados e encerra com código de saída 1 (9 casos propostos com status "NÃO CONFERIDO").
- **Rotina e Endpoint de Alertas Preventivos de Manutenção:**
  - `verificarAlertasManutencaoTabelasFiscais` em `apps/api/src/services/tabelas-fiscais.mjs` e endpoint `GET /api/v1/tabelas-fiscais/alertas-manutencao`.
  - Banner em tempo real no Painel do Contador (`apps/web/index.html` e `apps/web/js/app.js`) com alertas de tabelas a expirar em até 90 dias ou sem vigência para o ano subsequente.
- **Suíte de Testes da Fase 7.1 (`tests/fase7-1-vigencia-manutencao.test.mjs`):**
  - 13 testes garantindo a falha explícita dos 8 motores fiscais fora de vigência, a correta emissão de alertas e o código de saída do comando de pendências.

### Verificado
- **222 testes automatizados passando em 49 suítes com 0 falhas e 0 vulnerabilidades no `npm audit`.**

## [Fase 7 - Verificação e Homologação] - 24/10/2026

### Adicionado
- **Auditoria Técnica Honesta e Tabela de Integrações (`docs/auditorias/auditoria-fase7.md`):**
  - Mapeamento transparente do status real de cada integração oficial:
    - NFS-e Nacional: Assinatura XMLDSig C14N e montagem de DPS v1.00 100% operacionais e testadas; transmissão em Homologação/Mock até obtenção de mTLS com e-CNPJ real.
    - Serpro / Integra Contador: Catálogo completo REST e autenticação OAuth2 implementados; modo Homologação Híbrida / Mock ativo para proteção contra custos de tarifação.
    - SEFAZ / DF-e: Ingestão de XML, descompressão gzip, validação MOC 7.0 e parser de chave de 44 dígitos operacionais em arquivos locais; cliente SOAP de WebServices em planejamento.
    - OCR de Comprovantes: Parser heurístico determinístico com extração de valores em centavos, datas e CNPJ/CPF ativo; OCR visual (Tesseract/Google Cloud Vision) planejado para documentos escaneados.
    - Pix Bacen: Geração matemática determinística de BRCode estático TLV com CRC16-CCITT 100% compatível; falta integração com API Pix Dinâmica de PSP para baixa automática via webhook.
- **Matriz de Rastreabilidade Legal e Tributária (`docs/auditorias/auditoria-fase7.md`):**
  - Mapeamento exato de cada alíquota, base de cálculo, faixa e dedução utilizada nos motores fiscais com artigo de lei oficial, data de consulta e identificação de regras estimadas (alíquota patronal padrão de 28,8%, ICMS/ISS médios e alíquota de transição IBS/CBS da Reforma Tributária LC 214/2025).
- **Base Canônica de Casos de Referência Conferidos Manualmente (`tests/referencias/`):**
  - `tests/referencias/casos-conferidos.json`: 9 casos propostos de teste marcados estritamente como NÃO CONFERIDO (MEI misto comércio/serviços, Simples Nacional Anexo I Faixa 2, Lucro Presumido com presunção de 32% e retenções na fonte, Lucro Real anual com trava dos 30% em prejuízos fiscais).
  - `tests/referencias/casos-conferidos-template.csv`: Planilha estruturada modelo para expansão contínua de casos de teste por contadores e analistas fiscais.
  - `tests/referencias/README.md`: Guia de governança, preenchimento e critérios de validação manual.
- **Suíte de Testes de Referência Manual (`tests/referencias-manuais.test.mjs`):**
  - 6 testes automatizados que leem a base de casos conferidos e executam contra os motores fiscais oficiais (`apurarDasMei`, `apurarSimplesNacional`, `apurarIrpjCsllPresumidoTrimestral`, `calcularLalurLacs`), validando o batimento rigoroso em centavos.
- **Roteiro Operacional de Homologação nos PVAs Oficiais da Receita Federal (`docs/roteiros/validacao-sped.md`):**
  - Passo a passo detalhado de instalação, importação, validação e tratamento de avisos nos 4 programas validadores federais:
    1. PVA EFD ICMS/IPI (v3.1.x ou superior).
    2. PVA EFD Contribuições (v5.1.x ou superior).
    3. PVA ECD — Sped Contábil (v10.x ou superior).
    4. PVA ECF — Escrituração Contábil Fiscal (v10.x ou superior).
- **Checklist Rigoroso de Segurança e LGPD Pendente para Produção (`docs/auditorias/auditoria-fase7.md`):**
  - Separação auditada entre itens com evidência executada em testes (Cofre AES-256-GCM, 2FA TOTP RFC 6238, Multi-tenant com RLS, Hash SHA-256 e LGPD com revogação 403) e pendências reais de infraestrutura de produção (Cloud KMS/HSM, PostgreSQL gerenciado, WAF/Rate Limiting, Pentest externo, minutas jurídicas assinadas e nomeação de DPO).

### Verificado
- **209 testes automatizados passando em 44 suítes com 0 falhas e 0 vulnerabilidades no `npm audit`.**

## [Fase 6 - Obrigações Acessórias Avançadas, SPED, EFD-Reinf e Simulador Tributário] - 24/10/2026

### Adicionado
- **Gerador e Validador Pré-Transmissão EFD ICMS/IPI — SPED Fiscal (`packages/fiscal-core/src/sped-icms-ipi.mjs`):**
  - Geração completa no padrão oficial de delimitadores pipes (`|`) dos Blocos 0 (abertura, dados do contribuinte, contabilista, participantes 0150, itens 0200), C (documentos fiscais C100, itens C170, analítico C190), D (transportes D001/D990), E (apuração do ICMS E100/E110), 1 (outras informações 1010) e 9 (controle e encerramento).
  - Algoritmo de contagem rigorosa de linhas por registro para o Bloco 9 (`9900`, `9990`, `9999`).
  - Motor validador PVA local que checa estrutura de delimitadores, sequência obrigatória de blocos, batimento de totalizadores de documentos fiscais e consistência de apuração prévia à transmissão à RFB/SEFAZ.
- **Gerador e Validador Pré-Transmissão EFD Contribuições PIS/COFINS (`packages/fiscal-core/src/sped-contribuicoes.mjs`):**
  - Geração completa nos regimes Cumulativo (Lucro Presumido: PIS 0,65% e COFINS 3,00%) e Não-Cumulativo (Lucro Real: PIS 1,65% e COFINS 7,60%).
  - Blocos 0, A (serviços tomados e prestados A100/A170), C (mercadorias C100/C170), M (apuração consolidada PIS M200/M210 e COFINS M600/M610) e 9.
  - Validador pré-transmissão PVA integrado com relatório de inconsistências por registro.
- **Gerador e Validador ECD Diário Geral e ECF e-LALUR/e-LACS (`packages/fiscal-core/src/sped-ecd-ecf.mjs`):**
  - Geração do livro Diário Geral (Livro "G") da Escrituração Contábil Digital (ECD): Bloco 0, Bloco I (I010, I030 Termo de Abertura, I050 Plano de Contas, I155 Saldos, I200 Cabeçalho e I250 Partidas Contábeis em Partidas Dobradas) e Bloco J (Demonstrações Contábeis: Balanço Patrimonial J100, DRE J150 e Signatários J930 Contador e Administrador).
  - Validador contábil rigoroso da ECD: confirma a invariante fundamental de que a soma dos débitos é rigorosamente igual à soma dos créditos em cada lançamento I200/I250.
  - Memória de cálculo e apuração da Escrituração Contábil Fiscal (ECF): apuração do Lucro Real no e-LALUR e e-LACS com adições, exclusões e a trava legal dos 30% na compensação de prejuízos fiscais (Art. 510 RIR/2018).
- **Módulo Oficial EFD-Reinf (`packages/fiscal-core/src/reinf-service.mjs`):**
  - Montagem e validação dos eventos oficiais no padrão RFB:
    - `R-1000`: Informações do Contribuinte, classificação tributária e dados de contato.
    - `R-2010`: Retenção de Contribuição Previdenciária sobre serviços tomados com cessão de mão de obra (alíquota padrão de 11% ou 3,5% com desoneração).
    - `R-4020`: Pagamentos/créditos a pessoas jurídicas com retenção na fonte de IRRF (1,5%) e Contribuições Sociais Retidas na Fonte (CSRF: 4,65% = CSLL 1,00% + PIS 0,65% + COFINS 3,00%) conforme Lei 10.833/2003 e RIR/2018.
- **Motor de Cruzamentos Avançados e Risco de Malha Fiscal (`packages/fiscal-core/src/cruzamento-malha-fiscal.mjs`):**
  - Auditoria cruzada quadrangular: `XMLs de Saída e Entrada` × `Escrituração Contábil` × `Linhas do SPED Fiscal` × `Apurações Fiscais (DAS/DARF)`.
  - Detecção preditiva de omissão de receita (XMLs emitidos superiores ao faturamento apurado), notas fiscais sem contabilização e notas omitidas no arquivo SPED.
  - Cálculo do **Score de Risco de Malha Fiscal (0 a 100 pontos)** com categorização de severidade (`BAIXO`, `MEDIO`, `ALTO`, `CRITICO`) e plano de ação prescritivo para saneamento.
- **Módulo de Análise Gerencial e Demonstrações Financeiras (`packages/fiscal-core/src/analise-gerencial-service.mjs`):**
  - DRE Gerencial em cascata estruturada: Receita Bruta -> Deduções -> Receita Líquida -> CMV/CPV -> Lucro Bruto -> Despesas Operacionais -> EBITDA -> Depreciação -> EBIT -> Resultado Financeiro -> IRPJ/CSLL -> Lucro Líquido.
  - Balanço Patrimonial estruturado em Ativo Circulante, Ativo Não Circulante, Passivo Circulante, Passivo Não Circulante e Patrimônio Líquido.
  - Bateria de Indicadores Econômico-Financeiros: Liquidez Corrente, Seca, Imediata e Geral; Margem Bruta, Margem EBITDA e Margem Líquida; ROA e ROE; e Endividamento Geral.
- **Simulador Tributário Multirregime e Reforma Tributária LC 214/2025 (`packages/fiscal-core/src/simulador-tributario.mjs`):**
  - Comparativo determinístico lado a lado em centavos exatos:
    - **Simples Nacional:** cálculo por faixas dos Anexos I a V, segregação de receitas e aplicação automática da regra do Fator R (Folha / RBT12 >= 28% migra de Anexo V para Anexo III com CPP inclusa).
    - **Lucro Presumido:** bases presumidas de 8%/32% (IRPJ) e 12%/32% (CSLL), adicional de IRPJ, PIS 0,65% e COFINS 3,00% cumulativos, ICMS/ISS e encargos patronais de 28,8% sobre folha (CPP 20% + RAT + Terceiros).
    - **Lucro Real:** apuração sobre o lucro contábil LAIR projetado, PIS 1,65% e COFINS 7,60% não-cumulativos com apropriação de créditos sobre compras e insumos, créditos de ICMS na entrada e encargos sobre folha.
    - **Reforma Tributária (IBS/CBS):** simulação do regime de transição para 2026 (alíquota teste de 0,9% CBS e 0,1% IBS = 1,0%) e projeção do regime pleno no destino (alíquota neutra de ~26,5% com não-cumulatividade integral).
  - Recomendação automática do melhor regime tributário com demonstração da economia anual estimada em reais (R$).
- **Novos Endpoints RESTful (`apps/api/src/server.mjs`):**
  - `POST /api/v1/sped/efd-icms-ipi/gerar`: gera EFD ICMS/IPI e valida via PVA.
  - `POST /api/v1/sped/efd-contribuicoes/gerar`: gera EFD Contribuições e valida.
  - `POST /api/v1/sped/ecd/gerar`: gera ECD Livro Diário Geral a partir dos lançamentos contábeis.
  - `POST /api/v1/sped/validar-arquivo`: validador PVA agnóstico para qualquer arquivo SPED.
  - `GET /api/v1/sped/arquivos`: listagem e histórico de arquivos gerados e hashes.
  - `POST /api/v1/reinf/eventos/gerar`: geração de eventos R-1000, R-2010 e R-4020.
  - `GET /api/v1/reinf/eventos`: listagem dos eventos Reinf do cliente.
  - `POST /api/v1/auditoria/cruzamento-malha`: execução de conciliação quadrangular e pontuação de malha.
  - `GET /api/v1/gerencial/demonstracoes`: retorno de DRE, Balanço Patrimonial e Indicadores.
  - `POST /api/v1/fiscal/simulador-tributario`: cálculo e persistência do comparativo tributário.
  - `GET /api/v1/fiscal/simulacoes`: histórico de simulações salvas por empresa.
- **Interface Web do Painel do Contador (`apps/web/index.html` e `apps/web/js/app.js`):**
  - Três novas abas funcionais:
    - **📁 SPED & Obrigações:** geração de arquivos com 1 clique, prévia do texto em padrão pipe (`|`), validação PVA em tempo real e download de arquivo `.txt`.
    - **📈 Demonstrações & Indicadores:** painel com cards visuais de Liquidez, Margem EBITDA, Margem Líquida e ROE, DRE em cascata e Balanço Patrimonial estruturado.
    - **🧮 Simulador Tributário:** formulário para projeção de receitas e folha, cards comparativos lado a lado de Simples, Presumido, Real e Reforma Tributária, com regime vencedor e economia anual calculada.
- **Suíte de Testes da Fase 6:** 8 novas suítes de testes, elevando a suíte para **195 testes automatizados (100% passando, 0 falhas)**, mantendo cobertura de **91.66% de linhas e 94.67% de funções** e **0 vulnerabilidades de segurança**.

## [Fase 5 - Aplicativo Mobile e Portal do Cliente] - 24/10/2026

### Adicionado
- **Gerador de Pix Copia e Cola / BRCode EMVco (`packages/shared/src/pix-brcode.mjs`):**
  - Implementação do padrão EMVco TLV (Tag-Length-Value) e algoritmo CRC16-CCITT (polinômio 0x1021) conforme regulamentação do Banco Central do Brasil.
  - Geração automática de chave Pix, payload Copia e Cola e identificador de transação (TxID) para todas as guias de tributos (DAS, DARF, Guias Municipais).
- **Motor de OCR e Auto-Classificação de Documentos (`packages/fiscal-core/src/ocr-documentos-service.mjs`):**
  - Extração inteligente de dados-chave a partir de texto OCR/documentos: valores em centavos, datas no formato brasileiro (DD/MM/AAAA) e CNPJs/CPFs de favorecidos.
  - Auto-classificação determinística baseada em termos fiscais e bancários com índice de confiança: `NOTA_FISCAL`, `RECIBO`, `COMPROVANTE_PAGAMENTO` e `OUTROS`.
  - Deduplicação e prova de integridade através de hash SHA-256 de cada arquivo enviado.
- **Classificador de Saúde da Carteira e Semáforo do Contador (`packages/fiscal-core/src/carteira-semaforo.mjs`):**
  - Avaliação holística da saúde tributária e regulatória dos clientes da carteira com pontuação (0 a 100%) e semáforo:
    - 🔴 `VERMELHO` (Crítico): guias vencidas em atraso, procuração e-CAC expirada ou achados de auditoria graves.
    - 🟡 `AMARELO` (Atenção): guias a vencer nos próximos 5 dias, procuração vencendo em até 30 dias ou limite MEI acima de 80%.
    - 🟢 `VERDE` (Em Dia): todas as obrigações pagas, sem pendências regulatórias.
  - Painel gerencial consolidado com percentual global de conformidade do escritório.
- **Módulo de Aprovações Rápidas de Fechamento (`packages/integrations/src/aprovacoes-service.mjs`):**
  - Geração de links seguros com token criptográfico temporário de 72 horas (hash SHA-256 no banco).
  - Consulta pública segura sem expor dados internos do escritório.
  - Fluxo de aprovação ou rejeição formal pelo cliente com justificativa obrigatória, registro de IP de origem e nome do signatário.
  - Prova de integridade do documento via hash SHA-256 do payload JSON.
- **Central de Notificações Multicanal (`packages/integrations/src/notificacoes-service.mjs`):**
  - Motor automatizado de varredura de vencimentos com disparo multicanal (Push Mobile e WhatsApp) nos marcos D-5, D-1 e D-0.
  - Alertas automáticos sobre novos comunicados na Caixa Postal da RFB e documentos enviados.
  - Rastreamento completo de envio, leitura e histórico por cliente.
- **Endpoints RESTful da Fase 5 (`apps/api/src/server.mjs`):**
  - `GET /api/v1/cliente/dashboard`: resumo consolidado mobile com termômetro do MEI.
  - `GET /api/v1/cliente/guias`: guias com código de barras e Pix Copia e Cola dinâmico.
  - `POST /api/v1/cliente/documentos/upload`: upload de comprovantes com OCR e auto-classificação.
  - `GET /api/v1/cliente/documentos`: histórico de documentos enviados pelo cliente.
  - `POST /api/v1/cliente/solicitacoes` e `GET /api/v1/cliente/solicitacoes`: canal direto de dúvidas e chamados.
  - `POST /api/v1/cliente/aprovacoes/gerar-link`: geração de token de aprovação pelo contador.
  - `GET /api/v1/cliente/aprovacoes/token/:token`: consulta pública segura do fechamento a aprovar.
  - `POST /api/v1/cliente/aprovacoes/decidir/:token`: aprovação formal pelo empresário com IP e nome.
  - `POST /api/v1/notificacoes/disparar-vencimentos`: rotina de disparo D-5, D-1 e D-0.
  - `GET /api/v1/notificacoes` e `POST /api/v1/notificacoes/:id/marcar-lido`: consulta e confirmação de leitura.
  - `GET /api/v1/contador/carteira/semaforo`: painel de risco semafórico da carteira.
  - `POST /api/v1/cliente/nfse/rapida`: emissão ágil de NFS-e Nacional via mobile.
- **Portal do Cliente Mobile-First PWA (`apps/web/cliente.html` e `apps/web/js/cliente.js`):**
  - Interface responsiva dedicada ao empresário com termômetro MEI, pagamento de guias com Pix em 1 clique, captura de comprovantes pela câmera e emissor expresso de NFS-e.
- **Suíte de Testes da Fase 5:** 6 novas suítes de testes adicionadas, totalizando **172 testes automatizados** no monorepo (**100% de sucesso, 0 falhas**) com cobertura de **91.49% de linhas e 95.22% de funções**.

## [Fase 4 - Integra Contador / Serpro, Portais Governamentais e LGPD] - 24/10/2026

### Adicionado
- **Regra de Ouro LGPD e Consentimento Ativo (`gestao-procuracoes.mjs`, `serpro-facade.mjs`):**
  - Tabela `consentimento_acesso_cliente` rastreando autorização com data/hora, canal, IP de origem e usuário auditor.
  - Validador estrito `validarConsentimentoAtivo` exigido antes de qualquer consulta externa governamental.
  - Revogação imediata (`revogarConsentimento`) com bloqueio instantâneo (HTTP 403 / `CONSENTIMENTO_REVOGADO`).
- **Controle Rigoroso de Custos, Tarifação e Cotas (`controle-custos-serpro.mjs`):**
  - Contratos Serpro por escritório (`contrato_serpro`) com limites mensais de chamadas (`limite_mensal_chamadas`) e teto financeiro em reais (`limite_mensal_reais_centavos`).
  - Regra oficial de bilhetagem Serpro: apenas chamadas com sucesso HTTP 200 nos métodos `Consultar`, `Emitir` e `Declarar` são bilhetadas. Métodos de `Apoiar` e `Monitorar`, bem como códigos de erro (400, 401, 404, 429, 500, 503, 504), são isentos.
  - Alerta antecipado quando o consumo atinge a margem configurada (ex: 60%, 80%) e bloqueio automático imediato ao esgotar a cota (`COTA_SERPRO_EXCEDIDA`).
  - Relatório auditável com reconciliação matemática 100% exata entre chamadas registradas e faturadas.
- **Cliente HTTP Resiliente Serpro (`serpro-client.mjs`, `serpro-catalogo.mjs`):**
  - Autenticação OAuth2 oficial (Bearer Token JWT) com cache automático e renovação sob demanda (HTTP 401).
  - Headers governamentais obrigatórios: `Role-Type: TERCEIROS` e `X-Request-Tag` de rastreabilidade.
  - Tratamento resiliente de HTTP 429 (rate limiting com `Retry-After`) e HTTP 504 (timeout com semântica de estado indeterminado).
- **Gestão Proativa de Procurações Eletrônicas (`serpro-procuracoes.mjs`, `gestao-procuracoes.mjs`):**
  - Consulta oficial (`OBTERPROCURACAO41`) e sincronização das procurações ativas no e-CAC/Serpro.
  - Decomposição e normalização dos sistemas outorgados (`PGDASD`, `SITFIS`, `CAIXAPOSTAL`, `DCTF`, etc.).
  - Motor de alertas graduados por dias restantes: 60 dias (`ALERTA_60_DIAS`), 30 dias (`ALERTA_30_DIAS`), 7 dias (`ALERTA_7_DIAS`) e status `EXPIRADA`.
- **Serviços Oficiais do Simples Nacional — PGDAS-D (`serpro-pgdasd.mjs`):**
  - Geração oficial de DAS PGDAS-D (`GERARDAS12`) com retorno de número do documento, código de barras, vencimento oficial e PDF codificado em Base64.
  - Consulta de extrato de arrecadação do Simples Nacional (`CONSEXTRATO16`).
- **Caixa Postal da RFB com Monitoramento Isento (`serpro-caixapostal.mjs`):**
  - Consulta detalhada de mensagens oficiais (`MSGCONTRIBUINTE61`) e resolução dinâmica de variáveis em assuntos (`++VARIAVEL++`).
  - Monitoramento de novas mensagens sem tarifação (`INNOVAMSG63` em `/Monitorar`).
- **Situação Fiscal Consolidada — SITFIS (`serpro-sitfis.mjs`):**
  - Solicitação assíncrona de protocolo (`SOLICITARPROTOCOLO91` / Apoiar - Não tarifado) e emissão de relatório completo (`RELATORIOSITFIS92`).
  - Visão gerencial consolidada combinando guias pendentes, mensagens governamentais e alertas de procurações.
- **Fila Assíncrona com Idempotência e Backoff Exponencial (`fila-integracao.mjs`):**
  - Enfileiramento de requisições de integração com chave de idempotência estrita (`idempotency_key`), evitando chamadas duplicadas aos portais oficiais.
  - Retentativas automáticas com backoff exponencial e jitter determinístico para falhas temporárias de rede/governo.
- **Adaptador BrasilAPI de Serviços Públicos (`brasil-api.mjs`):**
  - Consulta unificada de dados cadastrais de CNPJ e calendário bancário de feriados nacionais.
- **Endpoints RESTful da Fase 4 (`apps/api/src/server.mjs`):**
  - `GET /api/v1/integracoes/serpro/contrato` e `POST /api/v1/integracoes/serpro/contrato`
  - `POST /api/v1/integracoes/consentimento` e `POST /api/v1/integracoes/consentimento/revogar`
  - `GET /api/v1/integracoes/procuracoes` e `POST /api/v1/integracoes/serpro/procuracoes/sincronizar`
  - `POST /api/v1/integracoes/serpro/pgdasd/gerar-das`
  - `GET /api/v1/integracoes/serpro/caixapostal` e `GET /api/v1/integracoes/serpro/caixapostal/novas`
  - `GET /api/v1/integracoes/serpro/consumo/relatorio`
  - `GET /api/v1/integracoes/situacao-fiscal`
  - `POST /api/v1/integracoes/fila/enfileirar` e `GET /api/v1/integracoes/fila/status`
- **Painel Operacional Web (`apps/web`):**
  - Nova aba "Integra Contador / RFB" com painel de cotas, gestão visual de consentimento LGPD, sincronização de procurações, emissor de DAS PGDAS-D e leitor da Caixa Postal RFB.
- **Suíte de Testes Automatizados da Fase 4:** 9 novas suítes de testes cobrindo autenticação Serpro, bilhetagem/cotas, LGPD/revogação, procurações, PGDAS-D, caixa postal, situação fiscal, fila assíncrona e endpoints RESTful, elevando a plataforma para **136 testes automatizados (100% de sucesso)** com **90.50% de cobertura de linhas e 94.41% de funções**.

## [Fase 3 - Emissão de Documentos Fiscais / NFS-e Nacional e Provedores Municipais] - 24/10/2026

### Adicionado
- **Cofre de Certificados Digitais A1 (`certificado-digital.mjs`):**
  - Armazenamento seguro de chaves privadas e arquivos PFX utilizando criptografia autenticada simétrica **AES-256-GCM** com IV exclusivo de 96 bits e Auth Tag de 128 bits.
  - Avaliação automatizada de vigência com alertas graduados aos 60 dias (`ALERTA_60_DIAS`), 30 dias (`ALERTA_30_DIAS`) e 7 dias (`ALERTA_7_DIAS`), além de bloqueio de uso para certificados com status `EXPIRADO`.
  - Gerador sintético de par de chaves RSA de 2048 bits para testes automatizados determinísticos.
- **Assinador Digital W3C XMLDSig (`assinador-xml.mjs`):**
  - Assinatura digital no padrão oficial ICP-Brasil / Sped com Canonicalização W3C C14N (`REC-xml-c14n-20010315`), SHA-256 DigestValue, RSA-SHA256 SignatureValue e nós `SignedInfo` / `Reference` / `KeyInfo` / `X509Certificate`.
  - Validador criptográfico independente que detecta adulteração do XML e garante conformidade antes da transmissão.
- **NFS-e Nacional — Ambiente de Dados Nacional ADN / CGNFS-e (`nfse-nacional.mjs`):**
  - Chave de acesso de 50 dígitos com cálculo de Dígito Verificador Módulo 11 (pesos de 2 a 9).
  - Montagem de XML da Declaração de Prestação de Serviços (DPS) em conformidade com o layout oficial do CGNFS-e.
  - Suporte antecipado à Reforma Tributária (EC 132/2023) com nós de IBS e CBS condicionados à data de vigência ($\ge 2027$).
- **Serviço Orquestrador de NFS-e e Idempotência Estrita (`nfse-emissor-service.mjs`):**
  - Ciclo de vida completo: Emissão, Consulta (por Chave de Acesso ou DPS/Série), Cancelamento com justificativa legal mínima de 15 caracteres e pedido assinado, e Substituição atômica de notas fiscais.
  - Controle estrito de idempotência na tabela `idempotencia_requisicao`: requisições com a mesma `Idempotency-Key` retornam a resposta original preservada com flag `reemissaoIdempotente: true`, impedindo notas duplicadas.
- **Provedores Municipais Legados (`provedor-abrasf.mjs`):**
  - Montagem de Lote RPS no padrão oficial ABRASF v2.04 para suporte a prefeituras legadas.
- **Endpoints RESTful da Fase 3 (`apps/api/src/server.mjs`):**
  - `POST /api/v1/certificados/upload`
  - `GET /api/v1/certificados`
  - `POST /api/v1/fiscal/nfse/emitir`
  - `GET /api/v1/fiscal/nfse/consultar`
  - `POST /api/v1/fiscal/nfse/cancelar`
  - `POST /api/v1/fiscal/nfse/substituir`
- **Painel Operacional Web (`apps/web`):**
  - Nova aba "Emissão NFS-e & Cofre A1" com consulta de vigência de certificados e emissor de DPS com assinatura XMLDSig em tempo real.
- **Suíte de Testes da Fase 3:** 6 novas suítes de teste adicionadas, alcançando **100 testes automatizados** no monorepo (100% de sucesso, 0 falhas) com cobertura global de 90.75% de linhas e 93.69% de funções.

## [Fase 2 - Motor Fiscal: MEI, Simples Nacional, Presumido e Real] - 24/10/2026

### Adicionado
- **Motor de Apuração do MEI (`apuracao-mei.mjs`):**
  - Apuração mensal do DAS-MEI (INSS 5% do Salário Mínimo + ICMS R$ 1,00 + ISS R$ 5,00 ou MEI Caminhoneiro).
  - Acompanhamento acumulado do teto anual (R$ 81.000,00) ou proporcional no ano de abertura (R$ 6.750,00/mês).
  - Alertas automatizados para 80% do limite, excesso de até 20% (desenquadramento no ano seguinte) e excesso superior a 20% (desenquadramento retroativo ao início do ano).
- **Motor do Simples Nacional (`apuracao-simples.mjs`):**
  - Cálculo e proporcionalização de RBT12 (1º mês: receita × 12; meses 2 a 11: média × 12).
  - Enquadramento nas 6 faixas dos Anexos I a V da LC 123/2006.
  - Cálculo oficial da Alíquota Efetiva: `(RBT12 * AliqNominal - Deducao) / RBT12`.
  - Mecanismo dinâmico do **Fator R** com alternância entre Anexo III ($\ge 28\%$) e Anexo V ($< 28\%$).
  - Segregação de receitas com desoneração seletiva: ICMS com Substituição Tributária (ICMS-ST), PIS/COFINS Monofásicos e ISS Retido na Fonte.
- **Motor do Lucro Presumido (`apuracao-presumido.mjs`):**
  - Bases presumidas por atividade (comércio/indústria 8%/12%, serviços 32%/32%, transporte 8%/16%).
  - IRPJ Trimestral (alíquota base de 15% + adicional de 10% sobre a parcela excedente a R$ 60.000,00).
  - CSLL Trimestral (alíquota de 9% sobre a base presumida).
  - PIS (0,65%) e COFINS (3,00%) mensais no regime cumulativo.
  - Compensação integral de retenções sofridas na fonte (IRRF e CSRF).
- **Calendário Fiscal e Ajuste de Dias Úteis (`calendario-fiscal.mjs`):**
  - Algoritmo de Páscoa (Meeus/Jones/Butcher) e derivação de todos os feriados móveis e fixos nacionais.
  - Regra de prorrogação (DAS dia 20 postergado para 1º dia útil seguinte) e antecipação (DARF PIS/COFINS dia 25 antecipado).
  - Vencimento do IRPJ/CSLL trimestral no último dia útil do mês subsequente ao encerramento do trimestre.
- **Endpoints RESTful da Fase 2 (`apps/api/src/server.mjs`):**
  - `POST /api/v1/fiscal/apuracao/mei`
  - `POST /api/v1/fiscal/apuracao/simples`
  - `POST /api/v1/fiscal/apuracao/presumido/trimestral`
  - `GET /api/v1/fiscal/apuracao/historico`
  - `GET /api/v1/fiscal/guias`
  - `POST /api/v1/fiscal/calendario/calcular-vencimento`
- **Suíte de Testes da Fase 2:** 8 novas suítes de teste totalizando 78 testes automatizados no projeto (100% de sucesso).

## [Fase 1 - Núcleo, Contabilidade e Auditoria de XML] - 24/10/2026

### Adicionado
- **Cadastro de Clientes e Empresas:** Modelo relacional completo com CNPJ/CPF, CNAE principal/secundários e município IBGE.
- **Plano de Contas Oficial:** Inicialização automática de plano de contas com vinculação a contas referenciais da ECD.
- **Lançamentos Contábeis em Partidas Dobradas:** Motor contábil que exige equilíbrio absoluto `Débitos == Créditos` e rejeita lançamentos desbalanceados.
- **Trava de Fechamento de Período:** Bloqueio atômico de novos lançamentos em competências com status `FECHADO` ou `BLOQUEADO`.
- **Balancete de Verificação:** Apuração de saldos devedores e credores com validação de consistência patrimonial.
- **Central de Ingestão de XMLs (DF-e Hub):** Testada e homologada com lote de 1.000 XMLs sintéticos, importando 950 únicos e detectando 50 duplicatas com relatório de rejeição detalhado.
- **Cérebro Fiscal (Auditoria Fiscal Automatizada):** Detecção de 100% dos defeitos fiscais plantados em testes: chaves inválidas (DV Módulo 11), notas não escrituradas, divergência de valores entre XML e contabilidade, e incoerências de CFOP.
- **Extrato Bancário e Conciliação:** Parser de extratos OFX bancários e algoritmo de sugestão de conciliação por valor e data aproximada.
- **Interface Operacional Web:** Painel responsivo em português brasileiro servido pelo backend (`apps/web`).
- **Adaptador de Serviços Públicos:** `BrasilApiAdapter` em `packages/integrations` para consulta de CNPJs e feriados nacionais.

## [Fase 0 - Fundação] - 24/10/2026

### Concluído
- Repositório monorepo estruturado em `D:\FiscalIntelligencePlatform` com branch `main`.
- Pacote `@fiscal/shared` com validação oficial de CPF, CNPJ, Chave de Acesso DF-e (44 dígitos + DV Módulo 11) e aritmética de centavos anti-float.
- Esquema de banco de dados relacional com `node:sqlite` no modo WAL e suporte completo a transações ACID.
- Implementação de multi-tenancy forte com isolamento comprovado por teste automatizado (`escritorio_id`).
- Sistema de tabelas fiscais versionadas por competência e vigência temporal (`tabela_fiscal`).
- Trilha de auditoria imutável (`log_auditoria`).
- API RESTful com autenticação, suporte a 2FA e endpoints de saúde, sessão, tabelas e auditoria.
- Suíte automatizada com 29 testes (100% aprovados) e cobertura > 96% em `shared`.
