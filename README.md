# Ecossistema de Inteligência Fiscal, Contábil e Automação

Plataforma profissional para processamento de documentos fiscais eletrônicos (DF-e), auditoria tributária contínua, conciliação, monitoramento de certidões e regularidade, e automações seguras.

---

## 🚀 Início Rápido

### Pré-requisitos
- **Node.js** `>= 22.0.0` (testado e homologado no Node.js v24.20.0 LTS)
- **npm** `>= 10.0.0`

### Instalação e Execução

```bash
# 1. Configurar variáveis de ambiente local
copy .env.example .env

# 2. Instalar dependências locais
npm install

# 3. Executar suíte de testes de validação fiscal
npm test

# 4. Iniciar a plataforma localmente (Backend + Frontend)
npm run dev
```

Acesse:
- **Painel Operacional:** [http://localhost:5173](http://localhost:5173)
- **API Health Check:** [http://127.0.0.1:3333/api/v1/health](http://127.0.0.1:3333/api/v1/health)

---

## 🏛️ Arquitetura do Ecossistema

O projeto é estruturado como um **Monólito Modular** composto por pacotes desacoplados e serviços integrados:

- `packages/fiscal-core`: Parsers seguros de XML (NF-e, NFC-e, CT-e, NFS-e), cálculo de hash canônico SHA-256 e validação de chaves.
- `packages/evidence-ledger`: Trilha criptográfica de auditoria categorizada em `OBSERVED`, `DERIVED` e `INFERRED`.
- `packages/rules-engine`: Motor de regras tributárias e detecção de inconsistências fiscais.
- `connectors/`: Conectores oficiais e simuladores com contratos estritos para SEFAZ e órgãos reguladores.
- `services/api`: Backend RESTful com banco de dados embutido `node:sqlite` (ACID) e isolamento multi-tenant.
- `apps/web`: Interface de usuário operacional em React, TypeScript e Tailwind CSS.
- `fixtures/`: Amostras sintéticas e cenários de testes oficiais.
- `docs/`: Documentação técnica completa estruturada em `especificacao/`, `fases/`, `auditorias/`, `conferencia/`, `roteiros/` e `pendencias/`.
- `archive/`: Arquivo de relatórios históricos e levantamentos superados.

---

## 🔒 Segurança e Privacidade

- **Proteção contra XXE (XML External Entity):** O parser rejeita e desabilita entidades externas e DTDs maliciosas.
- **Isolamento Multi-empresa:** Particionamento obrigatório por `organization_id`.
- **Zero Vazamento:** Nenhum segredo ou certificado digital é exposto em texto claro.
- **Conformidade LGPD:** Mascaramento e retenção controlada de dados pessoais.
