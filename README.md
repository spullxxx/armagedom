<img src="assets/cover.webp" alt="ARMAGEDOM" width="100%" />

# ARMAGEDOM

**Orquestração de Inteligência Artificial.** Um sistema de agentes que roda no meu computador e faz trabalho real de engenharia de software.

`Python` `Flask` `React 19` `Vite` `Tailwind 4` `Three.js` `Ollama` `ChromaDB` `SQLite` `LangGraph` `MCP`

---

## Sumário

- [Visão geral](#visão-geral)
- [Números](#números)
- [Arquitetura](#arquitetura)
- [Agentes](#agentes)
- [Busca e conhecimento (RAG)](#busca-e-conhecimento-rag)
- [Memória](#memória)
- [Skills aprendidas de livros](#skills-aprendidas-de-livros)
- [Modelos e failover](#modelos-e-failover)
- [Segurança e governança](#segurança-e-governança)
- [Interface](#interface)
- [Telas](#telas)
- [Stack completa](#stack-completa)
- [Status](#status)

---

## Visão geral

O ARMAGEDOM é um sistema de agentes de IA que roda direto no meu computador. Ele escreve código, revisa projetos, faz auditorias, controla o navegador e consulta minhas bases de conhecimento antes de responder.

Três ideias guiam o projeto:

1. **Nada acontece sem autorização.** Ações sensíveis param num gate e esperam aprovação humana.
2. **Toda resposta tem evidência.** O que o sistema afirma vem de código, documento ou memória que ele consegue citar.
3. **Mudanças pequenas e revisáveis.** Arquivos são alterados por diff, validados em sandbox e só então aplicados.

> Na prática, é o laboratório onde eu testo agentes antes de usá-los em projetos.

## Números

| | |
| --- | --- |
| Código do núcleo (Python) | ~39 mil linhas |
| Testes automatizados | 1.000+ |
| Skills em produção | 120+ |
| Agentes especializados | 13 |
| Provedores de modelo | Ollama, NVIDIA, APIs compatíveis com OpenAI, Gemini |

## Arquitetura

```text
Autoridade humana
    │
    ▼
Constituição (regras de governança aplicadas em runtime)
    │
    ▼
ODAN · orquestrador
    ├── Execution Manager ── ciclo de vida, orçamento, timeout, cancelamento, circuit breaker
    ├── Supervisor ───────── planeja e escolhe o especialista (LangGraph)
    │     ├── Code Researcher
    │     ├── Knowledge Researcher
    │     ├── Engineer
    │     └── Validator
    ├── Context Engine ───── monta o contexto de cada chamada dentro do orçamento de tokens
    ├── Model Gateway ────── roteamento e failover entre provedores
    ├── Capability Router ── ferramentas (arquivos, navegador, banco, MCP)
    ├── Knowledge Platform ─ RAG, grafo de conhecimento e skills
    ├── Memory Platform ──── memória de longo prazo
    ├── Approval Gate ────── aprovação humana para ações sensíveis
    ├── Event Bus / SSE ──── atividade em tempo real para a interface
    └── Evidence Store ───── registro do que foi feito e por quê
```

O **Execution Manager** e o **Supervisor** têm papéis separados de propósito: um controla *como* a execução roda (estado, limites, falhas), o outro decide *o que* fazer. Isso evita loops infinitos e execuções sem fim.

## Agentes

| Agente | Papel |
| --- | --- |
| **ODAN** | Orquestrador principal |
| **Supervisor** | Classifica o pedido e escolhe a rota |
| **Chat** | Conversa direta, sem ferramentas |
| **Knowledge** | Pesquisa nas bases de conhecimento |
| **Engineer** | Planeja e escreve mudanças de código |
| **Validator** | Confere o resultado antes de entregar |
| **Crítico / Analista** | Revisam riscos e qualidade da resposta |
| **Approval** | Para a execução e pede autorização |
| **Repair** | Tenta corrigir quando uma etapa falha |
| **Operation** | Executa ações no sistema e no navegador |

A **Sala dos Agents** mostra em tempo real o que cada um está fazendo e as mensagens trocadas entre eles.

## Busca e conhecimento (RAG)

A recuperação é **híbrida**, não depende só de embeddings:

- **BM25** para termos exatos (nomes de função, siglas, códigos de erro);
- **busca vetorial** no ChromaDB para significado;
- **fusão** dos dois resultados e **reranking** com FlashRank;
- filtro de **diversidade** para não repetir o mesmo trecho;
- **proveniência**: cada trecho usado na resposta guarda de onde veio.

O conhecimento também vira um **grafo**, que aparece na interface como uma **constelação 3D** navegável (Three.js), com centenas de nós.

## Memória

A memória de longo prazo não é só um histórico de conversa:

- **consolidação**: fatos repetidos viram uma memória só;
- **conflitos e substituição**: quando algo muda, a memória antiga é marcada como superada;
- **detecção de drift**: identifica quando o que está salvo não bate mais com a realidade;
- **ranking e reidratação**: só as memórias relevantes voltam para o contexto;
- armazenamento em **SQLite** e **ChromaDB**.

## Skills aprendidas de livros

Um pipeline transforma livros e materiais de estudo em habilidades que os agentes usam:

```text
documento → chunking → extração → compilação → validação estática → sandbox → avaliação → promoção
```

Cada skill passa por candidata antes de ir para produção. As skills se ligam num **grafo de habilidades**, e o agente escolhe qual usar pelo contexto do pedido.

## Modelos e failover

- **Modelos locais** via Ollama (roda sem internet).
- **Nuvem**: NVIDIA, Gemini e qualquer API compatível com OpenAI.
- **Failover automático**: se um provedor cai ou estoura o limite, a chamada passa para o próximo da cadeia sem interromper a tarefa.

## Segurança e governança

- **Constituição**: um documento de regras com hierarquia de autoridade, aplicado no runtime e não só como texto.
- **Approval Gate**: ações sensíveis (apagar, executar scripts, mexer fora do projeto) esperam aprovação.
- **Mutações por diff** com validação e sandbox antes de aplicar.
- **Circuit breaker** e **orçamento** por execução.
- **Módulos de segurança**: pipeline de pentest e varredura de vulnerabilidades, usados apenas em ambientes próprios ou autorizados.

## Interface

- Chat com anexos, histórico e modo supervisionado;
- **Workbench** com editor de código (Monaco);
- **Sala dos Agents** com atividade ao vivo via SSE;
- **Constelação 3D** do conhecimento;
- painéis de ferramentas, skills, execuções e livros indexados;
- tema claro e escuro.

## Telas

**Sala dos Agents**, com o supervisor, o pipeline e o estado de cada agente:

<img src="assets/sala-dos-agents.webp" alt="Sala dos Agents" width="100%" />

**Chat**, respondendo com as ferramentas disponíveis:

<img src="assets/chat.webp" alt="Chat" width="100%" />

**Skill Registry**, com as habilidades em produção:

<img src="assets/skills.webp" alt="Skill Registry" width="100%" />

## Stack completa

| Camada | Tecnologias |
| --- | --- |
| Backend | Python, Flask, LangGraph, SSE |
| Modelos | Ollama, NVIDIA, Gemini, APIs compatíveis com OpenAI |
| Dados | ChromaDB, SQLite, BM25, FlashRank |
| Ferramentas | MCP (cliente e servidor), Playwright, Serena |
| Frontend | React 19, TypeScript, Vite, Tailwind 4, Three.js, Monaco, Motion |
| Qualidade | pytest (1.000+ testes) |

## Status

Funcional e em evolução. O código não é aberto; este repositório documenta o projeto.

---

Feito por [Carlos Daniel](https://github.com/spullxxx) · [LinkedIn](https://www.linkedin.com/in/carlos-daniel-18787b362/) · [Instagram](https://www.instagram.com/carlaoodan/)
