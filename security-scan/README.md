# security-scan

Executa Snyk (scan de dependências) e SonarQube (qualidade de código) em sequência. Cada gate pode ser habilitado ou desabilitado independentemente.

## Uso

```yaml
- uses: D2nke/my_workflows/actions/security-scan@main
```

## Inputs

| Input | Obrigatório | Padrão | Descrição |
|-------|-------------|--------|-----------|
| `enable_snyk` | não | `true` | Habilita o scan de dependências com Snyk |
| `enable_sonar` | não | `true` | Habilita a análise de qualidade com SonarQube |
| `sonar_project_key` | não | `{owner}-{repo}` | Project key no SonarQube; gerado automaticamente se omitido |
| `coverage_report_paths` | não | `coverage.xml,**/target/site/jacoco/jacoco.xml` | Caminhos dos relatórios de cobertura |

## Secrets necessários no repositório

| Secret | Descrição |
|--------|-----------|
| `SNYK_TOKEN` | Token de autenticação do Snyk |
| `SONAR_TOKEN` | Token de autenticação do SonarQube/SonarCloud |

## Vars necessárias no repositório

| Var | Descrição |
|-----|-----------|
| `SONAR_ORG_KEY` | Chave da organização no SonarCloud |

## O que faz

1. Checkout com histórico completo (necessário para blame do SonarQube)
2. **Snyk** (se `enable_snyk: true`): instala Node, roda scan com threshold `high`, exibe resumo de vulnerabilidades
3. **SonarQube** (se `enable_sonar: true`): lista relatórios de cobertura, executa análise e aguarda o quality gate
4. Exibe resumo final dos gates executados

O Snyk roda com `continue-on-error: true` — falhas não bloqueiam o SonarQube nem o resumo.
