# Plano MVP — Versão Técnica (Requisitos + Arquitetura + Fluxo)

**Projeto:** Plataforma de avaliação estética facial (scan + recomendação assistida)
**Autoria:** Volund
**Data:** julho/2026

---

## 0. Princípio de design

> A plataforma é um **sistema de apoio à decisão clínica (CDSS)**, não um dispositivo de diagnóstico autônomo. Toda saída é **sugestão revisável por profissional habilitado**. Isso muda a arquitetura: o motor de recomendação é **auditável, explicável e sempre com humano no loop** (human-in-the-loop).

Estratégia recomendada (igual à adotada no INEA Águas Interiores): **frontend-first com dados/IA atrás de um provider mockado**. Constrói-se o fluxo e a UX primeiro; o motor de visão computacional e o motor de regras entram atrás de uma interface (`AnalysisProvider`) que começa mockada e depois é trocada pela implementação real, sem mexer na UI.

---

## 1. Escopo do MVP (o que entra / o que NÃO entra)

### Entra (MVP — Fase 1)
- Captura por **webcam** e/ou **upload** de foto, em 3 vistas: **frontal, lateral, colo**.
- **Guia de pose** (a "boneca"): overlay/silhueta indicando enquadramento correto.
- **Detecção facial 2D** (landmarks) com biblioteca open-source rodando **no navegador** (client-side).
- Cálculo de **métricas faciais** derivadas dos landmarks (ver §5).
- Formulário de **dados corporais**: peso, altura, IMC (auto), meta de perda de peso, idade, queixas.
- **Motor de regras** que cruza métricas faciais + dados corporais → lista priorizada de sugestões.
- **Revisão humana**: aceitar/editar/remover cada sugestão.
- **Relatório** (tela + exportação PDF/JSON) e marcação no avatar guia.

### NÃO entra no MVP (fases seguintes)
- Scan 3D real (TrueDepth/LiDAR/fotogrametria).
- "Antes/depois" gerado por IA generativa.
- Banco de pacientes multiusuário / prontuário / login clínico completo.
- Treinamento de modelo próprio de recomendação.

---

## 2. Requisitos

### 2.1 Requisitos Funcionais (RF)

| ID | Requisito | Prioridade |
|---|---|---|
| RF01 | Capturar imagem via webcam com preview em tempo real | Must |
| RF02 | Permitir upload de imagem como alternativa à webcam | Must |
| RF03 | Guiar as 3 poses (frontal/lateral/colo) com overlay da boneca | Must |
| RF04 | Validar enquadramento (rosto centralizado, iluminação mínima) antes de aceitar a captura | Should |
| RF05 | Detectar landmarks faciais e desenhar a malha sobre a imagem | Must |
| RF06 | Calcular métricas faciais (§5) a partir dos landmarks | Must |
| RF07 | Coletar peso, altura, meta de peso; calcular IMC e classificação OMS | Must |
| RF08 | Registrar queixas/áreas de interesse da paciente | Must |
| RF09 | Gerar sugestões priorizadas via motor de regras (§6) | Must |
| RF10 | Exibir justificativa ("por que") de cada sugestão | Must |
| RF11 | Permitir aceitar/editar/remover cada sugestão | Must |
| RF12 | Exibir disclaimers legais e termo de consentimento | Must |
| RF13 | Exportar relatório (PDF/JSON) | Should |
| RF14 | Persistir avaliação para acompanhamento (evolução) | Could (Fase 2) |
| RF15 | Simular antes/depois | Could (Fase 2/3) |

### 2.2 Requisitos Não Funcionais (RNF)

| ID | Requisito |
|---|---|
| RNF01 | **Privacidade por padrão:** no MVP, o processamento de imagem roda **no cliente** (imagem não sai do dispositivo sem consentimento explícito). |
| RNF02 | **LGPD:** imagem facial + dados de saúde = dados pessoais sensíveis (art. 5º, II). Exigir base legal (consentimento), minimização e criptografia em repouso/trânsito quando houver persistência. |
| RNF03 | **Desempenho:** detecção facial em tempo real ≥ 15 FPS em notebook mediano; análise completa < 3 s. |
| RNF04 | **Portabilidade:** rodar em Chrome/Edge/Safari desktop; ideal PWA para tablet/celular. |
| RNF05 | **Explicabilidade:** toda sugestão carrega regra + entradas que a dispararam (auditável). |
| RNF06 | **Acessibilidade:** contraste AA, textos em PT-BR, alvos de toque ≥ 44px. |
| RNF07 | **Resiliência:** se o modelo de CV não carregar (offline/CDN), cair para modo de captura simples sem quebrar. |

### 2.3 Regulatório / Ético (Brasil)

- **CFM/Conselhos de classe:** indicação e prescrição são atos privativos de profissional habilitado. O software não pode "prescrever" — só **sugerir para avaliação**.
- **ANVISA:** se o software fizer diagnóstico/decisão clínica autônoma, pode ser enquadrado como **SaMD (Software as a Medical Device)** — RDC 657/2022. Manter o posicionamento como **ferramenta de apoio não diagnóstica** reduz esse risco; validar com jurídico/regulatório antes de comercializar.
- **Consentimento de imagem** + finalidade explícita.
- Nunca prometer resultado; "simulação ilustrativa".

---

## 3. Arquitetura

```
┌──────────────────────────────────────────────────────────────┐
│                        FRONTEND (SPA/PWA)                      │
│                                                                │
│  UI (tela única do fluxo)                                      │
│   ├─ CaptureView  (webcam + overlay boneca + upload)          │
│   ├─ BodyDataForm (peso/altura/IMC/meta/queixas)             │
│   ├─ AnalysisView (malha facial + métricas)                   │
│   └─ ReportView   (sugestões revisáveis + export)            │
│                                                                │
│  ── AnalysisProvider (interface) ──────────────────────────  │
│      MockAnalysisProvider   ◄── começa aqui                   │
│      CVAnalysisProvider     ◄── depois (MediaPipe/face-api)   │
│                                                                │
│  Rules Engine (JSON de regras versionado)                     │
└───────────────────────────┬──────────────────────────────────┘
                            │ (Fase 2, opcional)
┌───────────────────────────▼──────────────────────────────────┐
│  BACKEND (Fase 2+)                                             │
│   ├─ API (auth clínica, RBAC)                                  │
│   ├─ Storage seguro de imagens (criptografado, LGPD)          │
│   ├─ Banco de avaliações (evolução no tempo)                  │
│   └─ Serviço de simulação antes/depois (IA)                   │
└──────────────────────────────────────────────────────────────┘
```

### Stack recomendada
- **Frontend:** Next.js (React) + TypeScript + Tailwind. (Alinha com o stack que a Volund já usa no INEA.)
- **Visão computacional (client-side):** **MediaPipe Face Landmarker** (via `@mediapipe/tasks-vision`) — 478 landmarks 3D, roda no navegador, grátis. Alternativa: **face-api.js** (68 pontos, mais leve, mais simples).
- **Motor de regras:** JSON versionado + avaliador simples em TS (sem engine pesada no MVP).
- **Export PDF:** `react-pdf` ou `pdfmake` client-side.
- **Backend (Fase 2):** o que a Volund já domina (Node/Nest ou .NET); storage em bucket com criptografia.

---

## 4. Contrato do `AnalysisProvider`

```ts
type Pose = 'frontal' | 'lateral' | 'colo';

interface Capture {
  pose: Pose;
  imageBitmap: ImageBitmap;      // frame capturado
  width: number; height: number;
}

interface FaceMetrics {
  // valores normalizados 0..1 (frações da largura/altura do rosto)
  jawlineDefinition: number;     // definição do contorno mandibular
  submentalFullness: number;     // "papada" (gordura submentoniana)
  chinProjection: number;        // projeção do mento (perfil)
  neckLaxity: number;            // flacidez do pescoço/colo
  nasolabialDepth: number;       // sulco nasogeniano
  underEyeHollow: number;        // olheira/depressão de tear trough
  foreheadDynamicLines: number;  // rugas dinâmicas (proxy p/ toxina)
  facialThirds: [number,number,number]; // proporção dos terços
  faceSymmetry: number;          // 1 = perfeitamente simétrico
  skinQuality: number;           // opcional (textura/poros) 0..1
}

interface AnalysisResult {
  metrics: FaceMetrics;
  landmarks?: Array<{x:number;y:number;z?:number}>;
  confidence: number;            // 0..1
  quality: { lighting:number; centering:number; sharpness:number };
}

interface AnalysisProvider {
  init(): Promise<void>;
  analyze(captures: Capture[]): Promise<AnalysisResult>;
}
```

- `MockAnalysisProvider` retorna métricas plausíveis (com leve variação) para tocar o fluxo.
- `CVAnalysisProvider` implementa com MediaPipe: extrai landmarks e **deriva** as métricas (§5).

---

## 5. Métricas faciais — como derivar dos landmarks

MediaPipe entrega 478 pontos 3D normalizados. As métricas são razões geométricas (robustas a escala). Exemplos:

| Métrica | Como calcular (proxy geométrico) |
|---|---|
| **Terços faciais** | distâncias verticais entre: linha do cabelo → glabela → subnasal → mento. |
| **Definição de mandíbula** | curvatura/ângulo do contorno mandibular (pontos do jawline); ângulo cervicomental no perfil. |
| **Papada (submental)** | no **perfil**: distância entre linha submentoniana e o pescoço; ângulo cervicomental > 120° sugere fullness. |
| **Projeção do queixo** | no **perfil**: posição do pogônio (mento) relativa à linha estética (ex.: linha de Riedel/Steiner). |
| **Flacidez de pescoço/colo** | textura/contorno na vista **colo**; ângulo do pescoço. |
| **Sulco nasogeniano** | profundidade do vinco entre asa nasal e canto da boca. |
| **Simetria** | espelhar landmarks no eixo médio e medir erro. |
| **Rugas dinâmicas** | pedir expressão (levantar sobrancelha) e medir deslocamento — proxy para indicação de toxina. |

> ⚠️ Estes são **proxies geométricos**, não medidas clínicas validadas. Para produto sério, calibrar contra avaliação de profissionais (ground truth) e documentar limitações.

---

## 6. Motor de regras (o coração do produto)

Regras declarativas em JSON, versionadas, **co-criadas com profissional habilitado**. Estrutura sugerida:

```json
{
  "version": "0.1.0",
  "rules": [
    {
      "id": "papada-imc-alto",
      "when": {
        "metrics.submentalFullness": { "gte": 0.6 },
        "body.imc": { "gte": 27 }
      },
      "suggest": {
        "procedure": "Redução de papada (enzimas / criolipólise submentoniana)",
        "area": "submentoniana",
        "priority": "media",
        "note": "IMC elevado: sugerir iniciar emagrecimento ANTES; reavaliar após meta de peso.",
        "sequence_after": "encaminhamento-nutricional"
      }
    },
    {
      "id": "queixo-projecao-baixa",
      "when": { "metrics.chinProjection": { "lte": 0.4 } },
      "suggest": {
        "procedure": "Preenchimento de mento (projeção de queixo)",
        "area": "mento", "priority": "media",
        "note": "Perfil com pouca projeção do pogônio."
      }
    },
    {
      "id": "toxina-testa",
      "when": { "metrics.foreheadDynamicLines": { "gte": 0.5 } },
      "suggest": {
        "procedure": "Toxina botulínica (terço superior)",
        "area": "testa/glabela", "priority": "alta",
        "note": "Rugas dinâmicas presentes."
      }
    },
    {
      "id": "bioestimulador-flacidez",
      "when": {
        "metrics.jawlineDefinition": { "lte": 0.45 },
        "metrics.neckLaxity": { "gte": 0.5 }
      },
      "suggest": {
        "procedure": "Bioestimulador de colágeno / avaliar fios de sustentação",
        "area": "contorno facial", "priority": "media"
      }
    },
    {
      "id": "encaminhamento-nutricional",
      "when": { "body.pesoParaPerder": { "gte": 8 } },
      "suggest": {
        "procedure": "Encaminhamento nutricional / avaliação de emagrecimento",
        "area": "corporal", "priority": "alta",
        "note": "Meta de perda de peso relevante impacta o resultado facial; priorizar antes de procedimentos de contorno."
      }
    }
  ]
}
```

**Avaliador (pseudo-TS):**
```ts
function evaluate(rules, ctx /* {metrics, body} */) {
  return rules
    .filter(r => matchesAll(r.when, ctx))
    .map(r => ({ ...r.suggest, ruleId: r.id, why: explain(r.when, ctx) }))
    .sort(byPriority);
}
```

- **Explicabilidade:** cada sugestão guarda `ruleId` + `why` (quais entradas dispararam).
- **Sequenciamento:** `sequence_after` monta a ordem sugerida (ex.: emagrecer → contorno → refinamento).
- **Segurança:** regras só sugerem categorias; **nunca** dose, produto ou marca.

---

## 7. Fluxo técnico (passo a passo)

```
1. init()            → carrega modelo CV (ou mock); pede permissão de câmera
2. capture(frontal)  → valida qualidade (luz/enquadramento) → guarda frame
3. capture(lateral)  → idem (boneca gira p/ perfil)
4. capture(colo)     → idem
5. analyze(captures) → landmarks → FaceMetrics
6. bodyForm          → {peso, altura} → imc = peso/(altura^2); pesoParaPerder
7. evaluate(rules, {metrics, body}) → sugestões priorizadas
8. review            → profissional aceita/edita/remove
9. report            → render + export (PDF/JSON) + marca avatar
10. (Fase 2) persist → storage seguro + histórico
```

---

## 8. Modelo de dados (MVP, client-side)

```ts
interface Assessment {
  id: string;
  createdAt: string;
  patient: { alias: string; idade?: number };   // sem PII desnecessária no MVP
  body: { pesoKg: number; alturaM: number; imc: number; classificacaoOMS: string; pesoParaPerder: number; };
  complaints: string[];
  captures: { pose: Pose; thumbnailDataUrl?: string }[];
  metrics: FaceMetrics;
  suggestions: Suggestion[];
  approvedByProfessional: boolean;
  consent: { imageUse: boolean; timestamp: string };
}
```

---

## 9. Testes / validação

- **Unit:** motor de regras (tabela de casos: entrada → sugestões esperadas).
- **CV:** conjunto de fotos rotuladas; comparar métricas derivadas vs. avaliação humana.
- **Usabilidade:** teste com profissionais reais (tempo de consulta, clareza das sugestões).
- **Viés:** validar detecção em diferentes tons de pele, idades e gêneros (modelos de CV têm viés conhecido).

---

## 10. Riscos e mitigação

| Risco | Mitigação |
|---|---|
| Enquadramento como dispositivo médico (ANVISA) | Posicionar como apoio não diagnóstico; revisão jurídica; disclaimers |
| Viés do modelo de CV | Validar em base diversa; expor `confidence` |
| Sugestão inadequada aceita sem revisão | Human-in-the-loop obrigatório; nada é "prescrição" |
| Vazamento de dados sensíveis | Processamento client-side no MVP; criptografia + base legal na Fase 2 |
| Expectativa irreal da paciente | "Simulação ilustrativa"; sem promessa de resultado |

---

## 11. Definição de Pronto (MVP)

- [ ] Captura das 3 vistas com guia de pose funcionando.
- [ ] Detecção facial real (MediaPipe) desenhando malha.
- [ ] Métricas derivadas + IMC calculado.
- [ ] Motor de regras retornando sugestões explicáveis.
- [ ] Revisão humana (aceitar/editar/remover).
- [ ] Relatório exportável.
- [ ] Disclaimers + consentimento visíveis.
- [ ] Validado com ≥ 2 profissionais reais.

> Protótipo de tela única (Fase 0) entregue em **simulador.html** — simula todo o fluxo sem backend.
