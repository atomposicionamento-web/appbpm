# Ferramenta de Otimização de Campanhas (Meta Business)

## Objetivo
Criar uma ferramenta que:
1. Conecte com o **Meta Business Manager** via login OAuth.
2. Leia campanhas ativas e históricas de contas de anúncio autorizadas.
3. Consolide métricas de performance.
4. Use IA para explicar, em linguagem simples, o que está acontecendo em cada campanha.
5. Sugira próximas ações priorizadas (otimização de orçamento, criativo, público, posicionamento e funil).

---

## Arquitetura recomendada (MVP)

### 1) Frontend
- Login com Meta (botão “Conectar com Meta”).
- Tela de seleção de Business / Ad Account.
- Dashboard por campanha e conjunto de anúncios.
- Cards de "Diagnóstico IA" + "Próxima melhor ação".

### 2) Backend API
Responsável por:
- OAuth Meta (troca de code por token).
- Armazenar tokens com criptografia.
- Consultar Graph API (campaigns, adsets, ads, insights).
- Normalizar métricas em modelo interno.
- Acionar módulo de IA para explicações e recomendações.

### 3) Banco de dados
Tabelas principais:
- `users`
- `meta_connections`
- `ad_accounts`
- `campaign_snapshots`
- `ai_diagnostics`
- `recommendation_actions`

### 4) Módulo de IA
- Entrada: métricas (CTR, CPC, CPM, CPA, ROAS, frequência, spend, conversões, etc.) + contexto (objetivo da campanha, janela de atribuição, baseline histórica).
- Saída:
  - Diagnóstico (o que piorou/melhorou e por quê).
  - Hipótese provável (ex.: fadiga criativa, público saturado, leilão caro).
  - Ação recomendada com prioridade, impacto esperado e risco.

---

## Fluxo de autenticação Meta (OAuth)

1. Usuário clica em “Conectar com Meta”.
2. Frontend redireciona para `https://www.facebook.com/vXX.X/dialog/oauth` com escopos adequados.
3. Usuário autoriza.
4. Meta retorna `code` para callback do backend.
5. Backend troca `code` por access token e, se aplicável, long-lived token.
6. Backend salva token criptografado e cria vínculo da conta.

### Escopos comuns (ajustar ao caso de uso)
- `ads_read`
- `ads_management` (somente se também for editar)
- `business_management`

> Importante: validar permissões realmente necessárias para reduzir fricção e risco de compliance.

---

## Coleta de métricas (Graph API)

Endpoints típicos:
- `/{ad-account-id}/campaigns`
- `/{campaign-id}/insights`
- `/{adset-id}/insights`
- `/{ad-id}/insights`

Métricas para MVP:
- `spend`
- `impressions`
- `reach`
- `clicks`
- `ctr`
- `cpc`
- `cpm`
- `frequency`
- `actions` / `conversions`
- `cost_per_action_type`
- `purchase_roas` (quando disponível)

Granularidade:
- Diário (`time_increment=1`) para detectar tendências.
- Janela comparativa (últimos 7d vs 7d anteriores; 30d vs 30d anteriores).

---

## Camada analítica antes da IA

Antes de chamar IA, calcular regras objetivas:
- Variações percentuais por métrica.
- Alertas de anomalia (ex.: CPC +35%, CTR -22%).
- Thresholds por objetivo (tráfego, leads, vendas).
- Score de saúde da campanha (0–100).

Exemplo de sinais:
- **Fadiga criativa**: frequência alta + CTR em queda + CPM estável.
- **Audiência saturada**: frequência alta + alcance estagnado.
- **Problema de conversão pós-clique**: CTR bom + CPA piorando.
- **Leilão mais competitivo**: CPM subindo forte em curto período.

Isso melhora consistência da IA e reduz alucinação.

---

## Prompt de IA (estrutura sugerida)

### System
“Você é um especialista em mídia paga Meta Ads. Explique resultados com objetividade, sem inventar dados, e sempre separar fatos de hipóteses.”

### User payload (JSON)
- Contexto da conta (segmento, ticket médio, objetivo da campanha).
- Métricas atuais e históricas.
- Alertas detectados pela camada analítica.
- Restrições operacionais (orçamento, criativos disponíveis, geos).

### Resposta esperada (JSON)
```json
{
  "summary": "...",
  "what_happened": ["..."],
  "likely_causes": ["..."],
  "recommended_actions": [
    {
      "action": "...",
      "priority": "high|medium|low",
      "expected_impact": "...",
      "risk": "...",
      "owner": "media_buyer|creative|analytics"
    }
  ],
  "confidence": 0.0
}
```

---

## Roadmap de entrega

### Fase 1 (2–3 semanas) — MVP funcional
- Login OAuth com Meta.
- Leitura de campanhas + insights básicos.
- Dashboard simples com KPIs.
- Diagnóstico IA textual por campanha.

### Fase 2 (2–4 semanas)
- Recomendação acionável com prioridade/impacto.
- Comparativos automáticos por período.
- Alertas proativos (email/Slack/WhatsApp).

### Fase 3
- Otimização semiautomática (playbooks).
- Testes A/B sugeridos pela IA.
- Benchmark interno por segmento/objetivo.

---

## Segurança e conformidade
- Criptografar tokens em repouso.
- Rotacionar chaves e segredos.
- Registro de auditoria por usuário/ação.
- LGPD: base legal, consentimento e retenção mínima.
- Evitar armazenar PII desnecessária.

---

## API interna sugerida

### `GET /auth/meta/start`
Inicia OAuth.

### `GET /auth/meta/callback`
Recebe `code`, troca token e salva conexão.

### `GET /accounts`
Lista ad accounts disponíveis para o usuário.

### `POST /sync/:accountId`
Sincroniza campanhas e insights.

### `GET /campaigns/:campaignId/diagnostic`
Retorna diagnóstico e recomendações de IA.

### `POST /campaigns/:campaignId/recommendations/:id/apply`
(para fase futura de automação assistida)

---

## Critérios de sucesso (KPIs da própria ferramenta)
- Tempo médio de diagnóstico por campanha.
- Redução de CPA após ações recomendadas.
- Aumento de ROAS nas campanhas otimizadas.
- Taxa de adoção das recomendações.
- Precisão percebida das explicações (feedback do usuário).

---

## Próximo passo prático
Se você quiser, no próximo passo eu já posso transformar este plano em um **esqueleto técnico executável** com:
- Backend (Node.js ou Python) + rotas OAuth Meta.
- Serviço de ingestão de insights.
- Módulo de IA com resposta estruturada.
- Dashboard inicial com tabela de campanhas e diagnóstico.
