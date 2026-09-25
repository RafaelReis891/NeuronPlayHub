# 🌊 A1_PRANCHA — Prancha do Mar

**Projeto:** NeuronPlay  
**Área:** A1 — Comunicação Aumentativa e Alternativa (CAA)  
**Minigame ID:** `A1_PRANCHA`  
**Plataforma:** Unity  
**Versão:** 1.0.0 (MVP)

---

## 📋 Índice

1. [Visão Geral](#-visão-geral)
2. [Objetivo Pedagógico](#-objetivo-pedagógico)
3. [Regras Fundamentais](#-regras-fundamentais)
4. [Arquitetura do Sistema](#-arquitetura-do-sistema)
5. [Fluxo de Funcionamento](#-fluxo-de-funcionamento)
6. [Níveis de Autonomia (N1–N5)](#-níveis-de-autonomia-n1n5)
7. [Sistema de Recompensas](#-sistema-de-recompensas)
8. [Telemetria](#-telemetria)
9. [Sistema de Feedback Visual](#-sistema-de-feedback-visual)
10. [Scripts e Responsabilidades](#-scripts-e-responsabilidades)
11. [Configuração dos ScriptableObjects](#-configuração-dos-scriptableobjects)
12. [Organização da Cena Unity](#-organização-da-cena-unity)

---

## 🌟 Visão Geral

**Prancha do Mar** é um minigame de Comunicação Aumentativa e Alternativa (CAA) baseado em pictogramas. A criança monta pequenas frases selecionando de 1 a 3 símbolos, ouve o áudio de cada um e, ao tocar em **FALAR**, escuta a sequência completa sendo vocalizada.

O minigame **não é um comunicador AAC completo** — é uma atividade estruturada com trials, recompensas e telemetria, focada em reforçar a comunicação funcional da criança.

### Estatísticas do MVP
- **19** Scripts modulares
- **40** Pictogramas na biblioteca
- **5** Níveis de Autonomia (N1-N5)
- **4** Trials padrão por sessão

### O que a criança faz:
1. Visualiza um conjunto de pictogramas (grid dinâmico)
2. Seleciona de 1 a 3 símbolos
3. Monta uma pequena frase na tira
4. Ouve o áudio de cada símbolo selecionado
5. Toca em **FALAR**
6. Ouve a sequência completa vocalizada
7. Recebe reforço visual e de recompensas (NeuronCoins + XP)
8. Repete o processo ou escolhe "Vamos parar aqui"

---

## 🎯 Objetivo Pedagógico

A atividade trabalha competências fundamentais da comunicação aumentativa:
- **Comunicação Funcional:** Expressão de necessidades e desejos de forma concreta.
- **Construção de Frases:** Montagem de estruturas simples (sujeito + verbo + objeto).
- **Expressão de Necessidades:** Vocabulário funcional (água, comida, banheiro, ajuda, sentimentos).
- **Reforço Positivo:** Toda tentativa de comunicação é validada e recompensada.
- **Autonomia Progressiva:** 5 níveis de suporte que reduzem gradualmente a ajuda visual.

---

## ⚖️ Regras Fundamentais

O minigame segue uma lógica pedagógica específica que **nunca deve ser violada**:

> **Princípio Central:**  
> `GAMEPLAY → COMUNICAÇÃO → REFORÇO`  
> **NUNCA:** `GAMEPLAY → ACERTO/ERRO → PUNIÇÃO`

1. **Sem Punição:** Nunca remover NeuronCoins, nunca mostrar "ERRADO" ou "X vermelho", nunca bloquear a atividade por escolha "incorreta".
2. **Daily Target é Bônus:** O alvo do dia é escolhido pelo adulto. Se alcançado, gera bônus. Se não, a criança recebe a recompensa normal do trial, sem penalidade.
3. **Comunicação Sempre Reforçada:** Toda frase vocalizada gera recompensa. Toda tentativa é válida.

---

## 🏗️ Arquitetura do Sistema

O minigame é composto por **19 scripts modulares**, desacoplados e com responsabilidade única.

```mermaid
graph TD
    PC["PranchaController<br/>Orquestrador Principal"]
    GM["PranchaGridManager<br/>Grid Dinâmico"]
    PS["PhraseStrip<br/>Tira de Frase"]
    VM["VoiceManager<br/>Áudio/TTS"]
    TM["PranchaTrialManager<br/>Controle de Trials"]
    DTM["DailyTargetManager<br/>Alvo do Dia"]
    THM["TargetHighlightManager<br/>Destaque N1-N5"]
    RM["PranchaRewardManager<br/>Recompensas"]
    TEL["PranchaTelemetry<br/>Telemetria"]
    NAV["NaviController<br/>Mascote"]
    FB["UIFeedbackManager<br/>Feedback Visual"]
    CFG["PranchaConfig"]
    SYM["SymbolData"]
    SC["SymbolCard"]
    SLOT["PhraseSlot"]
    NP["NeuronPlay<br/>Sistema Principal"]

    NP -->|Initialize config| PC
    CFG --> PC
    SYM --> CFG
    PC --> GM
    PC --> PS
    PC --> VM
    PC --> TM
    PC --> DTM
    PC --> THM
    PC --> RM
    PC --> TEL
    PC --> NAV
    PC --> FB
    GM --> SC
    PS --> SLOT
    FB --> SC
    FB --> PS
    RM -->|rewards| NP
    TEL -->|telemetry| NP

    classDef controller fill:#d55181,stroke:#fff,stroke-width:3px,color:#fff
    classDef manager fill:#4a9eff,stroke:#2e3949,stroke-width:2px,color:#fff
    classDef data fill:#c98500,stroke:#2e3949,stroke-width:2px,color:#fff
    classDef ui fill:#4ade80,stroke:#2e3949,stroke-width:2px,color:#fff
    classDef external fill:#a78bfa,stroke:#2e3949,stroke-width:2px,color:#fff,stroke-dasharray:5 5

    class PC controller
    class GM,PS,VM,TM,DTM,THM,RM,TEL,NAV,FB manager
    class CFG,SYM data
    class SC,SLOT ui
    class NP external
