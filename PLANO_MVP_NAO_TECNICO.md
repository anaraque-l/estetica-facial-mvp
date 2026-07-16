# Plano MVP — Versão Não Técnica

**Projeto:** Plataforma de avaliação estética facial com escaneamento e sugestão de procedimentos
**Público:** clínicas de estética, biomédicos esteta, dermatologistas, cirurgiões
**Data:** julho/2026

---

## 1. O que é, em uma frase

Um assistente que **fotografa/escaneia o rosto da paciente**, cruza com **dados corporais** (peso, altura, IMC, meta de emagrecimento) e **sugere um plano de procedimentos estéticos** para o profissional revisar e aprovar.

> ⚠️ **Importante:** a plataforma **não substitui o profissional**. Ela é um **apoio à decisão**. Quem indica, prescreve e executa o procedimento é sempre o profissional habilitado. A sugestão é um ponto de partida para a conversa com a paciente.

---

## 2. Para que serve (o problema que resolve)

Hoje a avaliação estética depende muito da experiência de quem olha. Isso gera:

- Avaliações que variam de profissional para profissional.
- Dificuldade de **mostrar para a paciente** o que se pretende fazer (a paciente não "enxerga" o que o profissional vê).
- Falta de **registro padronizado** (antes/depois, evolução).
- Consulta demorada e pouco visual.

A plataforma resolve isso com **padronização, apoio visual e um relatório pronto** em minutos.

---

## 3. Como funciona — o fluxo em 6 passos

```
[1] CAPTURA          [2] ANÁLISE          [3] DADOS
 Foto/scan do     →   Visão               →  Profissional
 rosto guiada         computacional          informa peso,
 por uma "boneca"     detecta pontos         altura, IMC,
 (frontal, perfil,    e regiões do           meta de perda
 colo p/ baixo)       rosto                   de peso, queixas

        ↓                                          ↓
[4] SUGESTÃO         [5] REVISÃO          [6] RELATÓRIO
 Motor de regras  →   Profissional        →  PDF / tela com
 cruza rosto +        edita, aprova           plano, prioridades
 corpo e propõe       ou descarta as          e antes/depois
 procedimentos        sugestões               simulado
```

### Passo a passo detalhado

1. **Captura guiada:** a tela mostra um **avatar/boneca** que demonstra a pose certa. A paciente (ou o profissional) posiciona o rosto conforme a boneca:
   - **Frontal** (rosto de frente)
   - **Lateral / perfil** (para avaliar queixo, papada, projeção)
   - **Colo para baixo** (pescoço e decote)

2. **Análise por visão computacional:** o sistema identifica automaticamente pontos do rosto (contorno do maxilar, queixo, olhos, sulcos, papada, flacidez do pescoço) e calcula proporções.

3. **Dados corporais:** o profissional digita peso, altura (o IMC é calculado sozinho), quantos quilos a paciente quer/precisa perder e as principais queixas.

4. **Sugestão automática:** com base no rosto + corpo, a plataforma sugere procedimentos, por exemplo:
   - *"IMC elevado + acúmulo de gordura submentoniana → considerar redução de papada (enzimas/criolipólise) **após** início do emagrecimento."*
   - *"Queixo pouco projetado no perfil → avaliar preenchimento de mento."*
   - *"Flacidez leve de contorno → avaliar bioestimulador de colágeno / lifting."*
   - *"Rugas dinâmicas na testa → avaliar toxina botulínica."*

5. **Revisão humana:** todas as sugestões vêm marcadas como *"sugestão para avaliação"*. O profissional **aceita, edita ou remove** cada uma.

6. **Relatório:** gera um resumo visual com o plano aprovado, a ordem sugerida dos procedimentos e um "antes/depois" simulado para conversar com a paciente.

---

## 4. O que entra e o que sai

| Entra (input) | Sai (output) |
|---|---|
| Fotos/scan (frontal, perfil, colo) | Lista de procedimentos sugeridos |
| Peso, altura, IMC, meta de peso | Prioridade e ordem sugerida |
| Queixas da paciente | Observações e alertas (ex.: "emagrecer antes") |
| Idade, histórico (opcional) | Relatório PDF + registro para acompanhamento |

---

## 5. O que é a "boneca"

É um **avatar guia** na tela. Ele:

- Mostra a **pose correta** de cada captura (vira de lado, aponta o queixo, etc.).
- Destaca a **região que está sendo avaliada** (ex.: acende a papada quando for avaliar papada).
- Serve depois para **mostrar o resultado** (marca no avatar onde entra cada procedimento: "aqui lift de papada", "aqui projeção de queixo").

No MVP a boneca é uma **silhueta/ilustração simples**. Em versões futuras pode virar um **avatar 3D** do próprio rosto da paciente.

---

## 6. Exemplos de procedimentos que o sistema sugere

- **Lift de papada** (redução de gordura submentoniana / flacidez do pescoço)
- **Projeção de queixo/mento** (preenchimento)
- **Toxina botulínica** (rugas dinâmicas: testa, glabela, "pés de galinha")
- **Preenchimento** (sulcos, olheiras, lábios, malar)
- **Bioestimuladores de colágeno** (flacidez leve/moderada)
- **Skinbooster / hidratação profunda** (qualidade de pele)
- **Fios de sustentação** (flacidez de contorno)
- **Encaminhamento nutricional** (quando IMC/meta de peso impactam o resultado)

---

## 7. Etapas do projeto (visão de cronograma)

| Fase | O que entrega | Prazo aproximado |
|---|---|---|
| **Fase 0 — Protótipo** | Tela única simulando o fluxo (esta entrega) | pronto |
| **Fase 1 — MVP funcional** | Captura real por webcam, detecção de rosto, motor de regras, relatório | 6–10 semanas |
| **Fase 2 — Clínico** | Antes/depois com IA, banco de pacientes, LGPD, multiusuário | +8–12 semanas |
| **Fase 3 — Avançado** | Scan 3D (celular/TrueDepth), evolução no tempo, integração com prontuário | conforme validação |

---

## 8. Cuidados importantes (não técnicos)

- **Consentimento:** a paciente precisa autorizar o uso da imagem (termo de consentimento + LGPD).
- **Responsabilidade profissional:** a indicação final é sempre humana. O texto da tela deixa isso explícito.
- **Sem promessa de resultado:** o "antes/depois" é uma **simulação ilustrativa**, não uma garantia.
- **Dados sensíveis:** foto de rosto + dados de saúde são dados sensíveis pela LGPD — precisam de armazenamento seguro e base legal.

---

## 9. Próximos passos sugeridos

1. Validar este protótipo (tela única) com 1–2 profissionais reais.
2. Ajustar as **regras de sugestão** junto de um profissional habilitado (é o coração do produto).
3. Definir se o MVP usa **foto 2D** (barato, roda em qualquer celular) ou **scan 3D** (mais caro, mais preciso).
4. Fechar o escopo da Fase 1.

> Ver detalhes de implementação em **PLANO_MVP_TECNICO.md** e opções de tecnologia em **PLATAFORMAS_E_APIS.md**.
