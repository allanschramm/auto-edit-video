# /gitflow

Garante que commits nunca vão direto para a main. Todo trabalho deve ser feito em branches com PRs.

## Regras obrigatórias

Antes de QUALQUER commit ou push, siga estas regras:

### 1. Nunca commitar na main

Antes de fazer `git commit`, SEMPRE verifique o branch atual:
```bash
git branch --show-current
```

Se estiver na `main`:
- **PARE IMEDIATAMENTE**
- Crie um branch antes de commitar:
  ```bash
  git checkout -b feat/<nome-descritivo>
  ```
- Só então faça o commit

### 2. Nomenclatura de branches

| Tipo | Prefixo | Exemplo |
|------|---------|---------|
| Feature nova | `feat/` | `feat/dry-run-mode` |
| Bug fix | `fix/` | `fix/whisper-language` |
| Refactor | `refactor/` | `refactor/extract-json-validation` |
| Docs | `docs/` | `docs/update-readme` |
| Chore/CI | `chore/` | `chore/add-ci` |

### 3. Fluxo de trabalho

```
1. Criar branch a partir da main
   git checkout main
   git pull
   git checkout -b feat/nome-da-feature

2. Fazer commits no branch
   git add <arquivos>
   git commit -m "feat: descrição"

3. Push do branch
   git push -u origin feat/nome-da-feature

4. Criar PR
   gh pr create --title "..." --body "..."

5. Merge via GitHub (nunca local)
   gh pr merge <numero> --squash
```

### 4. Checklist antes de cada commit

- [ ] Estou no branch correto? (`git branch --show-current` != `main`)
- [ ] Os testes passam? (`python -m pytest tests/ -v`)
- [ ] O lint passa? (`ruff check auto_edit/ tools/ tests/ --select E,F,W --ignore E501`)
- [ ] O ralph.sh está válido? (`bash -n ralph.sh`)

### 5. Se acidentalmente commitar na main

Não dê push. Faça:
```bash
# Salvar o hash do commit
COMMIT=$(git rev-parse HEAD)

# Voltar a main para o estado anterior
git reset --soft HEAD~1

# Criar branch e mover as mudanças
git checkout -b feat/nome-da-feature
git commit -c $COMMIT
```

Se já deu push na main, avisar o usuário antes de qualquer ação — force push na main é destrutivo.

## Quando usar esta skill

- Antes de começar qualquer trabalho novo
- Quando o usuário pedir para "commitar", "fazer commit", "push", ou "criar PR"
- Quando perceber que está na main com mudanças pendentes
