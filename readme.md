# LLM-Driven Differential Testing for Rust Compilers: A Maturity Assessment of gccrs

Este repositório contém o pacote de replicação e o dataset experimental
para o artigo científico que investiga o uso de Modelos Grandes de
Linguagem (LLMs) para sondar a maturidade do compilador `gccrs` por meio
de um fluxo de teste filtrado por oráculo.

## 📋 Resumo (Abstract)

Este trabalho propõe uma abordagem para testar compiladores Rust
utilizando LLMs para gerar código focado em funcionalidades instáveis
(*nightly*). A metodologia utiliza o compilador oficial `rustc` como um
oráculo de aceitação para filtrar casos de teste antes da submissão ao
`gccrs`. Os resultados de 1.403 instâncias geradas demonstram que,
embora as LLMs enfrentem desafios com as regras semânticas estritas do
Rust (3,7% de aceitação pelo oráculo), a abordagem é eficaz como uma
"sonda de maturidade", identificando gargalos críticos de infraestrutura
no compilador alvo.

------------------------------------------------------------------------

## 📁 Estrutura do Repositório

``` text
.
└── db/
    ├── database.db            # Banco de dados SQLite com o histórico completo
    └── database_export.sql    # Exportação SQL completa (Schema + Dados)
```

## Detalhes do Banco de Dados

O banco de dados SQLite rastreia a linhagem completa dos testes
realizados:

-   `prompts`: armazena as estratégias de instrução enviadas ao modelo.
-   `batches`: registra os lotes de execução e parâmetros de inferência.
-   `codes`: entidade base para cada instância de código gerada.
-   `code_retries`: histórico detalhado do "Retry Loop", incluindo
    mensagens de erro do `rustc` e correções da LLM.
-   `tests`: resultados finais da submissão ao compilador alvo
    (`gccrs`).

## 🛠️ Configuração Experimental

Para fins de reprodutibilidade, os dados contidos neste repositório
foram gerados sob as seguintes condições:

-   Modelo: Llama 3.1:8b (via Ollama)
-   Temperatura: 0.75
-   Contexto: 8192 tokens
-   Oráculo: rustc 1.91.0-nightly
-   Alvo: gccrs 16.0.0 (experimental)
-   Ambiente: Ubuntu Linux (Kernel 6.x)

## 🚀 Como Visualizar os Dados

### Usando SQLite CLI

Para carregar o banco e verificar a taxa de sucesso no alvo:

``` sql
sqlite3 db/database.db
SELECT COUNT(*) FROM tests WHERE is_successful = 1;
```

### Usando o Dump SQL

Para reconstruir o banco em outro ambiente:

``` bash
sqlite3 novo_banco.db < db/database_export.sql
```

## ⚖️ Licença

Este dataset e código estão licenciados sob a MIT License.\
O modelo Llama 3.1 está sujeito à Llama 3.1 Community License.

## 📝 Citação

Se você utilizar este dataset em sua pesquisa, por favor cite:

``` bibtex
@misc{magnabosco_gccrs_2026,
  author = {Fernando Magnabosco},
  title = {LLM-Driven Differential Testing for Rust Compilers: A Maturity Assessment of gccrs},
  year = {2026},
  publisher = {GitHub},
  journal = {GitHub Repository},
  howpublished = {\url{https://github.com/Fernando-Magnabosco/replication-package-gccrs-llm/tree/main/db}}
}
```
