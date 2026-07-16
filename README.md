# Avaliação Estética Facial — MVP

Protótipo de plataforma que **escaneia o rosto da paciente** (visão computacional) e o cruza com **dados corporais** (peso, altura, IMC, meta de perda de peso) para **sugerir um plano de procedimentos estéticos** — sempre como **apoio à decisão**, revisado por profissional habilitado.

## 🔗 Demo ao vivo (sem login)

- **Simulador:** https://anaraque-l.github.io/estetica-facial-mvp/simulador.html
- **Página inicial:** https://anaraque-l.github.io/estetica-facial-mvp/

## O que tem aqui

| Arquivo | Descrição |
|---|---|
| [`simulador.html`](simulador.html) | Tela única que simula todo o fluxo (captura → dados → análise → plano). Roda no navegador, sem backend. |
| [`PLANO_MVP_NAO_TECNICO.md`](PLANO_MVP_NAO_TECNICO.md) | Versão para leigo: fluxo em 6 passos, o que é a "boneca", exemplos de procedimentos, cronograma. |
| [`PLANO_MVP_TECNICO.md`](PLANO_MVP_TECNICO.md) | Requisitos (RF/RNF), arquitetura, contrato do provider, motor de regras, métricas faciais, regulatório. |
| [`PLATAFORMAS_E_APIS.md`](PLATAFORMAS_E_APIS.md) | Concorrentes, APIs grátis/pagas de visão computacional e opções de scanner 3D baratas. |

## Como rodar localmente

```bash
# qualquer servidor estático, ex.:
python -m http.server 8000
# depois abra http://localhost:8000
```

## Como funciona o fluxo

`Captura guiada (frontal/perfil/colo)` → `Análise por visão computacional` → `Dados corporais (IMC automático)` → `Motor de regras` → `Plano priorizado revisável` → `Relatório`

O motor de regras cruza métricas faciais + corpo. Ex.: *meta de 10 kg → encaminhamento nutricional antes do contorno*; *pouca projeção de queixo → preenchimento de mento*.

> **Observação técnica:** no protótipo, as métricas faciais são **simuladas** (proxies plausíveis). A estrutura para plugar detecção real (MediaPipe Face Landmarker, client-side) está descrita no plano técnico e isolada atrás de uma interface `AnalysisProvider`.

## ⚠️ Aviso

Ferramenta de **apoio à decisão** — não realiza diagnóstico e não prescreve. As sugestões devem ser revisadas, editadas ou descartadas por profissional habilitado. Simulação ilustrativa, sem promessa de resultado. Uso de imagem e dados de saúde exige consentimento (LGPD).
