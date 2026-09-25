<img src="assets/cover.webp" alt="ARMAGEDOM" width="100%" />

# ARMAGEDOM

**Orquestração de Inteligência Artificial.**

O ARMAGEDOM é um sistema de agentes de IA que roda direto no meu computador e faz trabalho real de engenharia de software: escreve código, revisa projetos e faz auditorias. Ele usa **RAG** para consultar minhas bases de conhecimento, que aparecem numa visualização 3D feita em Three.js.

Ele só executa ações que eu autorizo e altera arquivos com mudanças pequenas, fáceis de revisar. Se um provedor de IA cai, ele troca sozinho entre modelos locais no Ollama e APIs na nuvem.

Também controla o navegador, extrai dados de páginas e tem módulos de segurança para pentest e varredura de vulnerabilidades. A memória fica salva em ChromaDB e SQLite, então ele lembra do que já fez.

> Na prática, é o laboratório onde eu testo agentes antes de usá-los em projetos.

## Principais recursos

| Área | Recursos |
| --- | --- |
| **IA & Busca** | RAG / busca semântica · Constelação 3D (Three.js) · Automação de navegador |
| **Modelos** | Ollama · APIs em nuvem · Failover multi-provedor |
| **Segurança** | Constituição de autorização · Mutações por diff · Pentest · Varredura de vulnerabilidades |
| **Memória & Dados** | ChromaDB · SQLite · Grafos de habilidades |

## Como funciona

- **Supervisor** classifica cada pedido e escolhe o especialista certo (chat, conhecimento, operação, engenharia).
- **Sala dos Agents** mostra em tempo real o que cada agente está fazendo e a conversa entre eles.
- **Gate de aprovação**: ações sensíveis param e esperam minha autorização antes de rodar.
- **Skill Registry**: habilidades extraídas de livros e estudos são compiladas, validadas e promovidas para produção.

## Telas

<img src="assets/sala-dos-agents.webp" alt="Sala dos Agents" width="100%" />

<img src="assets/skills.webp" alt="Skill Registry" width="100%" />

## Status

Funcional e em evolução. O código não é aberto; este repositório documenta o projeto.

---

Feito por [Carlos Daniel](https://github.com/spullxxx) · [LinkedIn](https://www.linkedin.com/in/carlos-daniel-18787b362/) · [Instagram](https://www.instagram.com/carlaoodan/)
