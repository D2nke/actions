# release-tag

Gera e publica uma git tag semver calculada a partir do tipo do último commit seguindo [Conventional Commits](https://www.conventionalcommits.org) e [semver.org](https://semver.org).

## Uso

```yaml
- uses: D2nke/my_workflows/actions/release-tag@master
```

Não requer inputs nem secrets.

## Outputs

| Output | Descrição |
|--------|-----------|
| `version` | Nova tag criada (ex: `v1.2.0`) |
| `bump` | Tipo de incremento aplicado: `major`, `minor` ou `patch` |

## Permissões necessárias no job

```yaml
permissions:
  contents: write
```

## Regras de bump

| Condição no commit | Incremento | Exemplo |
|--------------------|------------|---------|
| `!` após o tipo ou `BREAKING CHANGE` no corpo | `major` | `feat!: remove API v1` |
| Tipo `feat` | `minor` | `feat: add dark mode` |
| Qualquer outro tipo (`fix`, `perf`, `refactor`, `docs`, `chore`, `ci`, `test`, `style`, `build`, `revert`) | `patch` | `fix: null pointer on login` |

Se nenhuma tag semver existir no repositório, parte de `v0.0.0`.

## Exemplos de bump

| Último commit | Tag anterior | Nova tag |
|---------------|-------------|----------|
| `feat: add search` | `v1.2.3` | `v1.3.0` |
| `fix: typo in label` | `v1.2.3` | `v1.2.4` |
| `feat!: redesign API` | `v1.2.3` | `v2.0.0` |
| `chore: update deps` | `v0.0.0` | `v0.0.1` |

## O que faz

1. Faz checkout com histórico completo (`fetch-depth: 0`)
2. Busca a última tag semver existente (fallback `v0.0.0`)
3. Lê o subject e o corpo do último commit
4. Determina o tipo de bump pelas regras acima
5. Cria uma tag anotada e faz push para o repositório remoto
