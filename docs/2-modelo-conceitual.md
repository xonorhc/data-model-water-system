# MODELO CONCEITUAL

## Modelo Conceitual usando [Mermaid](https://mermaid.js.org/)

Modelo conceitual simplificando as entidades e focando em como elas se relacionam de maneira abstrata, sem detalhamento técnico:

```mermaid
erDiagram
    Ativo {
        string Nome
        string Descricao
        string Tipo
        string Status
        string Localizacao
    }

    Manutencao {
        string Tipo
        string Descricao
        string Data
        string Custo
    }

    Equipe {
        string Nome
        string Responsavel
    }

    CentroCusto {
        string Descricao
    }

    Municipio {
        string Nome
    }

    Ativo ||--o| Manutencao: "realiza"
    Manutencao ||--o| Equipe: "realizada por"
    Ativo ||--o| CentroCusto: "pertence a"
    Ativo ||--o| Municipio: "localizado em"

```
