# security-scan

Executa Snyk (scan de dependências) e SonarQube (qualidade de código) em sequência. Cada gate pode ser habilitado ou desabilitado independentemente.

## Uso

```yaml
- uses: D2nke/actions/security-scan@main
  with:
    snyk_token: ${{ secrets.SNYK_TOKEN }}
    sonar_token: ${{ secrets.SONAR_TOKEN }}
    sonar_org_key: ${{ vars.SONAR_ORG_KEY }}
```

## Inputs

| Input | Obrigatório | Padrão | Descrição |
|-------|-------------|--------|-----------|
| `snyk_token` | não | — | Token de autenticação do Snyk (necessário se `enable_snyk: true`) |
| `sonar_token` | não | — | Token de autenticação do SonarQube/SonarCloud (necessário se `enable_sonar: true`) |
| `sonar_org_key` | não | — | Chave da organização no SonarCloud (necessário se `enable_sonar: true`) |
| `enable_snyk` | não | `true` | Habilita o scan de dependências com Snyk |
| `enable_sonar` | não | `true` | Habilita a análise de qualidade com SonarQube |
| `sonar_project_key` | não | `{owner}-{repo}` | Project key no SonarQube; gerado automaticamente se omitido |
| `coverage_report_paths` | não | `coverage.xml,**/target/site/jacoco/jacoco.xml` | Caminhos dos relatórios de cobertura |

## O que faz

1. Checkout com histórico completo (necessário para blame do SonarQube)
2. **Snyk** (se `enable_snyk: true`): instala Node, roda scan com threshold `high`, exibe resumo de vulnerabilidades
3. **SonarQube** (se `enable_sonar: true`): lista relatórios de cobertura, executa análise e aguarda o quality gate
4. Exibe resumo final dos gates executados

O Snyk roda com `continue-on-error: true` — falhas não bloqueiam o SonarQube nem o resumo.
