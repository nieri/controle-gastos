0# Spec — Spec 0: Versionamento com Git e conexão ao GitHub

**Projeto:** Controle de Gastos Pessoais
**Stack:** Python 3.11+ · FastAPI · SQLite · Pydantic

## Convenções gerais do projeto (valem para todas as specs)
- API REST em FastAPI; respostas em JSON; datas no formato ISO `YYYY-MM-DD`.
- Persistência em SQLite num arquivo local do projeto; a estrutura do banco é criada na inicialização se não existir.
- Valores monetários tratados com 2 casas decimais; `tipo` sempre `"entrada"` ou `"saida"`.
- Erros de validação retornam HTTP 422 com corpo `{ "erro": "<mensagem clara>" }`.
- Código organizado em módulos (rotas, modelos, acesso a dados) e versionado no Git a cada interação.

---

**Objetivo (por quê):** colocar o projeto sob controle de versão desde o primeiro minuto e publicá-lo num repositório no GitHub, para que cada spec implementada vire um commit rastreável e o código fique seguro fora da máquina local.

**Contexto:** a pasta do projeto ainda não é um repositório Git. Existem apenas `specs/` (Specs 0–6) e `docs/` (roteiro de prompts). Nenhum código da aplicação foi escrito. Esta spec é pré-requisito de todas as outras.

**Requisitos funcionais:**
1. Inicializar um repositório Git local na raiz do projeto, com branch principal chamada `main`.
2. Criar um `.gitignore` adequado a projetos Python que ignore, no mínimo:
   - ambiente virtual (`.venv/`, `venv/`);
   - cache Python (`__pycache__/`, `*.pyc`, `.pytest_cache/`);
   - banco SQLite local (`*.db`, `*.sqlite`, `*.sqlite3`);
   - arquivos de ambiente com segredos (`.env`, `.env.*`, exceto `.env.example`);
   - arquivos de sistema/editor (`.DS_Store`, `.vscode/`, `.idea/`);
   - configurações locais do Claude Code (`.claude/settings.local.json`).
3. Criar um `README.md` mínimo com: nome do projeto, descrição em uma frase, stack e um aviso de que as instruções de execução virão com a Spec 1.
4. Fazer o **commit inicial** contendo `.gitignore`, `README.md`, `specs/` e `docs/`.
5. Criar um repositório remoto no GitHub chamado `controle-gastos` e conectá-lo como `origin`.
6. Enviar (`push`) a branch `main` para o GitHub, configurando o rastreamento (`upstream`).

**Critérios de aceite:**
- `git status` na raiz do projeto mostra "working tree clean" após o commit inicial.
- `git branch --show-current` retorna `main`.
- `git log --oneline` mostra exatamente um commit, com mensagem descritiva (ex.: `chore: inicializa repositório com specs e documentação`).
- `.DS_Store` **não** aparece em `git ls-files`.
- Criar um arquivo de teste `teste.db` ou `.env` na raiz e rodar `git status` não o lista como não rastreado (depois apagar o arquivo de teste).
- `git remote -v` mostra `origin` apontando para o repositório `controle-gastos` no GitHub.
- `git status` informa que a branch `main` está atualizada com `origin/main`.
- O repositório no GitHub exibe o `README.md` e as pastas `specs/` e `docs/`.

**Decisões em aberto (decidir antes de implementar e registrar no `CLAUDE.md` quando ele for criado na Spec 1):**
- **(a) Visibilidade do repositório:** público ou privado? *Recomendação: privado* enquanto o projeto estiver em construção; tornar público ao final, se for portfólio.
- **(b) Forma de autenticação com o GitHub:** GitHub CLI (`gh auth login`), SSH ou HTTPS com token pessoal? *Recomendação: GitHub CLI* — autentica pelo navegador, configura o Git automaticamente e permite criar o repositório remoto pelo terminal (`gh repo create`). Exige instalar o `gh` (ex.: `brew install gh`).
- **(c) Padrão de mensagens de commit:** *Recomendação: Conventional Commits* (`feat:`, `fix:`, `chore:`, `docs:`, `test:`), com um commit por spec implementada.

**Fora de escopo:** código da aplicação, `CLAUDE.md` (criado na Spec 1), GitHub Actions/CI, proteção de branch, issues, pull requests, licença e deploy.

**Restrições técnicas:**
- Nenhum segredo (token, senha, chave) pode ser gravado em arquivos do projeto ou em commits.
- A autenticação interativa (`gh auth login` ou geração de chave SSH) é feita pelo próprio usuário no terminal; o Claude apenas orienta e verifica (`gh auth status` / `ssh -T git@github.com`).
- Antes do commit inicial, revisar `git status` para garantir que só os arquivos esperados estão sendo versionados.
