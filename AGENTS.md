# Regras Permanentes para Agentes de Desenvolvimento (AGENTS.md)

**Projeto:** Ecossistema de Inteligência Fiscal, Contábil, Dados, IA e Automação  
**Diretiva Suprema:** Conformidade com a Verdade Técnica, Segurança e Rastreabilidade  

---

## 1. Princípios Inegociáveis

1. **Evidência Antes da Conclusão:** Nunca afirme que um teste passou, que uma integração está ativa ou que um recurso foi implementado sem comando executado e saída observável.
2. **Proteção Contra XXE e Entidades Externas:** Todo processamento de XML deve obrigatoriamente desabilitar parsing de DTDs externas e entidades não seguras.
3. **Isolamento Multi-empresa (Multi-tenancy):** Nenhuma query de seleção, alteração ou exclusão pode ser executada sem a chave de partição da organização (`organization_id`).
4. **Zero Segredos em Código:** Arquivos `.pfx`, `.p12`, senhas, tokens de API ou dados de pessoas físicas nunca devem ser gravados em commits, repositórios públicos ou logs de execução.
5. **Human-in-the-Loop em Operações Tributárias:** Nenhum agente autônomo de IA tem autorização para emitir, cancelar ou transmitir documentos fiscais a órgãos do governo sem aprovação explícita e auditada de um usuário humano qualificado.
6. **Classificação Rigorosa da Informação:**
   - `OBSERVED`: Fato diretamente presente na fonte.
   - `DERIVED`: Resultado obtido por cálculo matemático ou transformação determinística.
   - `INFERRED`: Hipótese probabilística gerada por modelo ou heurística (obrigatório pontuar confiança de 0.0 a 1.0).
7. **Espaço em Disco:** Nunca utilize o disco `C:\` para builds, downloads de pacotes ou repositórios auxiliares, devido ao limite crítico de espaço do sistema operacional. Todo o trabalho deve residir em `D:\FiscalIntelligencePlatform`.
