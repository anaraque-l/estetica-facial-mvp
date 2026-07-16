# Plataformas, APIs e Scanners — o que já existe e como construir

**Data:** julho/2026
**Objetivo:** mapear soluções prontas (para comprar/integrar ou se inspirar) e ferramentas gratuitas/baratas para **construir** o seu próprio scanner facial.

> Preços mudam e a maioria dessas empresas trabalha com **cotação sob demanda (custom)**. Confirme sempre direto no site antes de decidir.

---

## 1. Plataformas prontas de simulação/avaliação estética (concorrentes / referência)

Essas já fazem, no todo ou em parte, o que você descreveu. Servem para: (a) entender o estado da arte, (b) decidir *comprar vs. construir*.

| Plataforma | O que faz | Foco | Modelo |
|---|---|---|---|
| **Crisalix** ([crisalix.com](https://www.crisalix.com/en)) | Reconstrução e simulação **3D/4D** de face e corpo a partir de fotos; simula rinoplastia, botox, preenchimento etc. Usa IA + visão computacional + AR. | Cirurgia plástica e estética | SaaS + captura por foto/app |
| **Perfect Corp — AI Aesthetic Simulator / Face Reshaping** ([perfectcorp.com](https://www.perfectcorp.com/business/solutions/online-service/aesthetic-simulation)) | Simulação de botox, preenchimento, before/after facial; roda em site e no atendimento presencial. | Estética facial, varejo de beleza | SaaS / API B2B |
| **Quantificare (LifeViz)** ([quantificare.com](https://www.quantificare.com)) | Câmeras + software de imagem **3D**, medição e simulação de resultados. | Clínicas/derma/plástica | Hardware + software |
| **Canfield Scientific (VISIA / VECTRA)** ([canfieldsci.com](https://www.canfieldsci.com)) | **VISIA** = análise de pele padrão-ouro (manchas, poros, rugas, UV); **VECTRA H2/M3** = imagem 3D e simulação. | Derma e plástica premium | Hardware clínico (caro) |
| **FaceReveal** ([face-reveal.com](https://face-reveal.com/aesthetic-treatment-simulation-tool/)) | Análise facial por IA + **recomendação automática** de tratamentos minimamente invasivos. Muito próximo do seu conceito. | Estética facial | SaaS |
| **Haut.AI** ([haut.ai](https://haut.ai/)) | **Análise de pele por IA** (150+ parâmetros) + recomendação; API para marcas. | Skincare / diagnóstico de pele | API/SaaS enterprise |
| **Revieve** ([revieve.com](https://www.revieve.com/)) | Suite de diagnóstico de pele, consultor por IA, recomendação de produto; lançou **LiveAR** (visualização de pele em tempo real, 2026). | Beleza/varejo | SaaS |
| **GlamAR** ([glamar.io](https://www.glamar.io)) | Try-on e simuladores de estética; publica comparativos do setor. | Beleza/varejo | SaaS/API |

**Leitura de mercado:** o "concorrente" mais parecido com seu pedido (rosto → recomendação de procedimento) é **FaceReveal** e **Crisalix**; a parte de **pele** é **Haut.AI/Revieve/VISIA**. Nenhum cruza *dados corporais (peso/IMC/meta) + rosto* de forma explícita — **esse é o seu diferencial**.

---

## 2. APIs / SDKs para **construir** o scanner (visão computacional)

### 2.1 Grátis / open-source (recomendado para o MVP)

| Ferramenta | O que entrega | Onde roda | Custo |
|---|---|---|---|
| **MediaPipe Face Landmarker** ([Google](https://ai.google.dev/edge/mediapipe/solutions/vision/face_landmarker)) | **478 landmarks 3D** + blendshapes (expressões) em tempo real | Navegador (JS), Android, iOS, Python | **Grátis** (Apache-2.0) |
| **TensorFlow.js face-landmarks-detection** ([npm](https://www.npmjs.com/package/@tensorflow-models/face-landmarks-detection)) | Wrapper do MediaPipe FaceMesh (468–478 pontos) em JS | Navegador | **Grátis** |
| **face-api.js** ([GitHub](https://github.com/justadudewhohacks/face-api.js)) | 68 landmarks, detecção, idade/gênero/expressão | Navegador | **Grátis** (mais simples/leve) |
| **OpenCV** ([opencv.org](https://opencv.org)) | Processamento de imagem, detecção, geometria | Python/C++/JS(wasm) | **Grátis** |
| **DeepFace** ([GitHub](https://github.com/serengil/deepface)) | Análise facial (idade, emoção, etc.) em Python | Servidor/Python | **Grátis** |
| **Google ML Kit — Face Detection** ([developers.google.com](https://developers.google.com/ml-kit/vision/face-detection)) | Detecção + contornos + landmarks em mobile | Android/iOS on-device | **Grátis** |
| **Apple ARKit — Face Tracking** ([developer.apple.com](https://developer.apple.com/documentation/arkit)) | Malha facial **1.220 pontos** + 52 blendshapes via TrueDepth | iPhone/iPad com FaceID | **Grátis** (precisa hardware) |

➡️ **Escolha para o MVP:** **MediaPipe Face Landmarker** (via `@mediapipe/tasks-vision`). Roda no navegador, é grátis, dá 478 pontos 3D e mantém a imagem no dispositivo (bom para LGPD). É o que o `simulador.html` usa como opção real.

### 2.2 Pagas / comerciais (quando escalar)

| Ferramenta | Uso | Modelo de preço |
|---|---|---|
| **Banuba Face AR SDK** ([banuba.com](https://www.banuba.com/facear-sdk/beauty-ar)) | Beautificação, try-on, AR facial on-device (iOS/Android/Web) | Custom, por **MAU** × plataformas × features |
| **Haut.AI API** ([haut.ai](https://haut.ai/)) | Análise de pele (150+ params) para recomendação | Enterprise, sob cotação |
| **Face++ (Megvii)** ([faceplusplus.com](https://www.faceplusplus.com)) | Detecção, landmarks (106 pts), análise facial via API cloud | Freemium + pago por chamada |
| **Luxand FaceSDK** ([luxand.com](https://www.luxand.com)) | Landmarks e reconhecimento, on-premise | Licença |
| **AWS Rekognition** ([aws.amazon.com/rekognition](https://aws.amazon.com/rekognition/)) | Detecção facial cloud (atributos, não malha densa) | Por imagem |
| **Azure AI Face** ([azure.microsoft.com](https://azure.microsoft.com/en-us/products/ai-services/ai-face)) | Detecção facial cloud (acesso a alguns recursos é **restrito/aprovação**) | Por transação |

> ⚠️ APIs **cloud** enviam a imagem para o servidor do fornecedor — implicação de LGPD e consentimento. Para dados sensíveis de saúde, prefira **on-device** (MediaPipe/ML Kit/ARKit).

---

## 3. Formas de escanear o rosto em 3D (métodos + opções gratuitas)

"Scanner 3D" é só **uma família** de métodos. O que importa para estética é capturar profundidade/volume (perfil, projeção de queixo, papada) — e há várias formas, muitas **de graça e sem hardware especial**. O custo muda conforme o **método de captura**.

### 3.1 Panorama dos métodos

| Método | Como funciona | Precisa hardware especial? | Custo |
|---|---|---|---|
| **1. Foto única + IA (3DMM)** | Uma IA reconstrói a malha 3D a partir de **uma foto comum** | ❌ Qualquer celular/webcam | **Grátis** ⭐ |
| **2. FaceMesh (MediaPipe)** | 478 pontos com coordenada Z a partir da câmera 2D | ❌ Qualquer webcam | **Grátis** ⭐ |
| **3. Fotogrametria** | Várias fotos ao redor do rosto → software monta a malha | ❌ Qualquer câmera | **Grátis** |
| **4. Câmera de profundidade (structured light)** | Projetor IR de pontos (TrueDepth/FaceID) | ✅ iPhone/iPad com FaceID | Grátis (app) + aparelho |
| **5. LiDAR / ToF** | Sensor mede tempo de retorno da luz | ✅ iPhone Pro, alguns Android | Grátis (app) + aparelho |
| **6. Estéreo (2 câmeras)** | Duas lentes calculam profundidade por paralaxe | ⚠️ Celular com 2 câmeras | Grátis |
| **7. NeRF / Gaussian Splatting** | Vídeo curto → IA reconstrói a cena 3D | ❌ Qualquer câmera (processa depois) | Grátis/freemium |
| **8. Scanner dedicado barato** | Hardware USB de baixo custo | ✅ | Baixo a médio |

### 3.2 Melhores opções gratuitas, por método

**⭐ Método 1 — Foto única + IA (o mais barato; recomendado para começar)**
Reconstrói um rosto 3D a partir de **uma única foto normal** — sem iPhone caro, sem sensor. Open-source:
- **DECA** / **EMOCA** — rosto 3D + expressão de uma foto (modelo FLAME).
- **MICA** — foco em geometria precisa.
- **3DDFA_V2** — leve e rápido, bom para pose/perfil.
- **Deep3DFaceRecon** (Microsoft) — clássico baseado em 3DMM.
- **FLAME** — o "esqueleto" 3D de cabeça usado por baixo da maioria.

**⭐ Método 2 — MediaPipe FaceMesh (já usado no `simulador.html`)**
Os 478 pontos do MediaPipe **já têm coordenada Z** (profundidade relativa). Não é scan clínico, mas mede **projeção de queixo, ângulo cervicomental (papada) e simetria** de graça, com qualquer webcam. É o "3D dos pobres" — suficiente para o MVP.

**Método 3 — Fotogrametria (grátis)**
- **Meshroom** (AliceVision) — pipeline completo, open-source.
- **COLMAP** — padrão acadêmico (structure-from-motion).
- **openMVG / MicMac** — bibliotecas open-source.
- No celular: **KIRI Engine** (grátis com limite: até 70 fotos/scan, 3 exports/semana) e **Polycam** (freemium). Técnica: dar a volta de orelha a orelha mantendo distância constante.

**Métodos 4 e 5 — Depth camera / LiDAR (app grátis, precisa do aparelho)**
- **iPhone/iPad com FaceID** (structured light) ou **iPhone Pro** (LiDAR). Apps grátis/baratos: **Heges**, **Scandy Pro**, **3D Scanner App**, **SureScan 3D**. Exportam `.ply/.obj`. Qualidade bem acima da foto 2D — e a clínica provavelmente já tem um iPhone.

**Método 6 — Estéreo (2 câmeras)**
- Celulares com câmera dupla permitem estimar profundidade por paralaxe (OpenCV `StereoBM/StereoSGBM`). Menos usado para rosto; mais trabalhoso de calibrar.

**Método 7 — NeRF / Gaussian Splatting**
- **Luma AI** e **Polycam** (Gaussian Splatting): grava-se um vídeo curto girando ao redor do rosto e a nuvem reconstrói. Freemium. Ótimo para "antes/depois" imersivo — exagero para o MVP.

**Método 8 — Hardware clínico (padrão-ouro, caro)**
- **VISIA / VECTRA (Canfield)**, **LifeViz (Quantificare)**. Dezenas de milhares. Só faz sentido em fase avançada.

➡️ **Custo-benefício:** para estética você **não precisa de scanner 3D real no MVP**. Ordem recomendada:
1. **Agora (grátis, zero hardware):** MediaPipe FaceMesh (métricas de perfil/queixo/papada) + opcionalmente **foto única + IA (DECA/3DDFA)** para uma malha 3D bonita de mostrar à paciente.
2. **3D real quando a clínica tiver iPhone:** app TrueDepth/LiDAR (Heges/Scandy) exportando `.ply/.obj`.
3. **Precisão premium:** hardware clínico (VECTRA/LifeViz) — só na fase avançada.

> **Pulo do gato:** para avaliar **papada e projeção de queixo**, o que importa é o **perfil 2D bem enquadrado** — e isso o MediaPipe mede de graça. O 3D "de verdade" agrega mais no *marketing/visual* do que na decisão clínica.

---

## 4. Como construir o seu (resumo prático)

### Caminho A — MVP 2D no navegador (recomendado, mais barato)
```
Webcam → getUserMedia → <video>
      → MediaPipe Face Landmarker (@mediapipe/tasks-vision, CDN)
      → 478 landmarks → derivar métricas geométricas
      → motor de regras (JSON) + dados corporais
      → relatório
```
- Stack: Next.js/React + TypeScript + Tailwind.
- Tudo client-side → imagem não sai do dispositivo → LGPD-friendly.
- Custo de infra ~ zero no início.

### Caminho A+ — 3D visual grátis, sem hardware (foto única + IA)
```
1 foto comum → modelo 3DMM (DECA / 3DDFA_V2) → malha 3D do rosto
      → mostra volume/perfil para a paciente + reforça métricas
```
- Zero hardware especial; roda a partir de uma única foto.
- Bom para o "uau" visual sem precisar de iPhone com TrueDepth.

### Caminho B — 3D com celular (mais preciso, Fase 3)
```
iPhone TrueDepth (ARKit) → malha 3D + blendshapes
      → app nativo/RN → medições de perfil/volume
      → mesmas regras + simulação antes/depois
```

### Caminho C — Comprar/integrar
- Se o objetivo é ir rápido ao mercado e a parte 3D/simulação é crítica: integrar **Perfect Corp** ou **Crisalix** (white-label/API) e focar seu esforço no **diferencial** (cruzamento rosto + corpo + regras próprias).

---

## 5. Recomendação final

1. **Construa o MVP no Caminho A** (MediaPipe no navegador) — barato, rápido, privado.
2. **Seu diferencial** não é a detecção facial (isso é commodity grátis) — é o **motor de regras que cruza rosto + peso/IMC/meta + queixas**, co-criado com profissional. Invista aí.
3. **Pele** (manchas, poros): se precisar, integre **Haut.AI** em vez de reconstruir.
4. **3D real** só quando houver validação de mercado (Caminho B com iPhone).
5. Trate **LGPD/ANVISA/CFM** desde o início (ver PLANO_MVP_TECNICO.md §2.3).

---

### Fontes
- Crisalix — https://www.crisalix.com/en
- Perfect Corp (Aesthetic Simulator / Face Reshaping) — https://www.perfectcorp.com/business/solutions/online-service/aesthetic-simulation
- FaceReveal — https://face-reveal.com/aesthetic-treatment-simulation-tool/
- Haut.AI — https://haut.ai/
- Revieve — https://www.revieve.com/
- Banuba Face AR SDK (pricing) — https://www.banuba.com/banuba-pricing-face-ar-sdk
- MediaPipe Face Landmarker — https://ai.google.dev/edge/mediapipe/solutions/vision/face_landmarker/web_js
- TensorFlow.js face-landmarks-detection — https://www.npmjs.com/package/@tensorflow-models/face-landmarks-detection
- Apple ARKit (Face Tracking) — https://www.sidekickinteractive.com/uncategorized/apples-arkit-for-scanning-and-tracking-faces/
- Apps de scan 3D (Scandy/Heges/SureScan) — https://www.artec3d.com/learning-center/best-3d-scanner-apps
- Meshroom (AliceVision, fotogrametria open-source) — https://alicevision.org/#meshroom
- COLMAP (structure-from-motion) — https://colmap.github.io/
- KIRI Engine (scan 3D no celular, freemium) — https://www.kiriengine.app/
- Luma AI (NeRF / Gaussian Splatting) — https://lumalabs.ai/
- DECA (reconstrução 3D de rosto de foto única) — https://github.com/yfeng95/DECA
- 3DDFA_V2 (rosto 3D leve de foto única) — https://github.com/cleardusk/3DDFA_V2
- FLAME (modelo 3D de cabeça) — https://flame.is.tue.mpg.de/
