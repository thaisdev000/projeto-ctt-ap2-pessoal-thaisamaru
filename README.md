[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/A5LZMZew)

# Projeto CTT-AP2: Documentação do Roadmap de Go

Este repositório contém a documentação técnica sobre a linguagem Go (Golang), cobrindo desde os conceitos fundamentais de sintaxe até tópicos avançados como concorrência e testes automatizados. O projeto foi desenvolvido utilizando a ferramenta **Zensical** para geração de sites estáticos e implantado automaticamente via **GitHub Pages**.

## 👥 Integrantes da Equipe
* [Nome Completo do Integrante 1] - [RM/Matrícula]
* [Nome Completo do Integrante 2] - [RM/Matrícula]
* [Nome Completo do Integrante 3] - [RM/Matrícula]
* [Nome Completo do Integrante 4] - [RM/Matrícula]

---

## ⚙️ Fluxo de Trabalho (Workflow) da Equipe

Para garantir a organização do código e evitar conflitos nos arquivos de Markdown (`.md`), a equipe adotou o seguinte fluxo de desenvolvimento:

1. **Divisão de Módulos:** Os 10 tópicos do roadmap de Go foram distribuídos entre os integrantes para a produção de conteúdo técnico e exemplos práticos de código.
2. **Desenvolvimento Local:** Cada integrante editou e estruturou seus arquivos correspondentes dentro do diretório `docs/` utilizando o **VS Code** e **Git Bash**.
3. **Sincronização via Git:** O versionamento foi feito de forma contínua através do terminal com comandos padronizados:
   * `git add .` para palpar as alterações.
   * `git commit -m "docs: descrição do ajuste"` para registrar o progresso.
   * `git push origin main` para enviar as atualizações ao repositório centralizado.

---

## 👀 Processo de Revisão e Garantia de Qualidade

Durante o projeto, enfrentamos o desafio de garantir que a sintaxe customizada do Markdown fosse interpretada corretamente pelo servidor de hospedagem. O processo de revisão envolveu:

* **Validação de Sintaxe:** Inspeção manual detalhada para garantir o correto fechamento de blocos de código Go (```` ```go ````) e blocos de comandos em Bash.
* **Compatibilidade do Zensical:** Adaptação de componentes de alerta (`:::info` / `> [!NOTE]`) para o formato universal de **Blockquotes padrão** (`>`), assegurando que notas informativas e dicas visuais fossem renderizadas de forma limpa e legível no site final, sem códigos quebrados.
* **Inspeção de Listas:** Ajuste no espaçamento obrigatório de marcadores (`> * ` e `> - `) para garantir a correta quebra de linha em formato de viñetas uma abaixo da outra.

---

## 🤖 Arquitetura do Workflow do GitHub Actions

A implantação (deploy) do site é 100% automatizada através de uma esteira de **CI/CD (Integração e Entrega Contínuas)** configurada no GitHub Actions.

### Como o Pipeline Funciona por Baixo dos Panos:
1. **Gatilho (Trigger):** Sempre que um integrante realiza um `git push` na branch `main`, o GitHub detecta a alteração e ativa o fluxo automaticamente.
2. **Ambiente Isolado (Runner):** O Actions inicializa uma máquina virtual Linux (`ubuntu-latest`) nos servidores da nuvem.
3. **Instalação de Dependências:** O robô faz o download do código, configura o ambiente necessário (Node.js/Python dependendo da base do projeto) e instala a ferramenta Zensical.
4. **Compilação (Build):** O gerador lê os 10 arquivos Markdown dentro da pasta `docs/` e os converte em arquivos HTML, CSS e JavaScript estáticos prontos para a web.
5. **Hospedagem (Deploy):** Os arquivos gerados são transferidos de forma segura para a branch de publicação, atualizando o link público em tempo real.

---

## 🔗 Links do Projeto
* **Repositório do Código Fonte:** [Coloque aqui o link do seu GitHub, ex: [https://github.com/thaisdev000/](https://github.com/thaisdev000/)...]