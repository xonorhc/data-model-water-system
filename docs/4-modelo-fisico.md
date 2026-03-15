# MODELO FÍSICO

## Modelo Físico em SQL ([PostgreSQL](https://www.postgresql.org/) )

Estrutura SQL para o modelo físico baseado no [PostgreSQL](https://www.postgresql.org/) para o sistema de gestão de ativos de saneamento:

```sql
-- Criação da tabela para os Tipos de Ativo
CREATE TABLE Tipo_Ativo (
    id_tipo SERIAL PRIMARY KEY,
    descricao VARCHAR(255) NOT NULL
);

-- Criação da tabela para os Sistemas (Captação, Adutoras, etc.)
CREATE TABLE Sistema (
    id_sistema SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL
);

-- Criação da tabela para os Centros de Custo
CREATE TABLE Centro_Custo (
    id_centro_custo SERIAL PRIMARY KEY,
    descricao VARCHAR(255) NOT NULL
);

-- Criação da tabela para os Municípios
CREATE TABLE Municipio (
    id_municipio SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL
);

-- Criação da tabela para as Equipes
CREATE TABLE Equipe (
    id_equipe SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    responsavel VARCHAR(255)
);

-- Criação da tabela para os Ativos
CREATE TABLE Ativo (
    id_ativo SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    descricao TEXT,
    status VARCHAR(50) NOT NULL,
    id_tipo INT NOT NULL,
    id_sistema INT NOT NULL,
    id_centro_custo INT NOT NULL,
    id_municipio INT NOT NULL,
    localizacao TEXT,
    vida_util INT, -- Vida útil estimada em anos
    valor_contabil DECIMAL(15, 2), -- Valor contábil do ativo
    CONSTRAINT fk_tipo FOREIGN KEY (id_tipo) REFERENCES Tipo_Ativo(id_tipo),
    CONSTRAINT fk_sistema FOREIGN KEY (id_sistema) REFERENCES Sistema(id_sistema),
    CONSTRAINT fk_centro_custo FOREIGN KEY (id_centro_custo) REFERENCES Centro_Custo(id_centro_custo),
    CONSTRAINT fk_municipio FOREIGN KEY (id_municipio) REFERENCES Municipio(id_municipio)
);

-- Criação da tabela para as Manutenções
CREATE TABLE Manutencao (
    id_manutencao SERIAL PRIMARY KEY,
    tipo VARCHAR(255) NOT NULL, -- Ex: Preventiva, Corretiva
    descricao TEXT NOT NULL,
    data DATE NOT NULL,
    custo DECIMAL(15, 2), -- Custo da manutenção
    id_ativo INT NOT NULL,
    id_equipe INT NOT NULL,
    CONSTRAINT fk_ativo FOREIGN KEY (id_ativo) REFERENCES Ativo(id_ativo),
    CONSTRAINT fk_equipe FOREIGN KEY (id_equipe) REFERENCES Equipe(id_equipe)
);

-- Criação da tabela para a Localização do Ativo
CREATE TABLE Localizacao (
    id_localizacao SERIAL PRIMARY KEY,
    endereco VARCHAR(255),
    municipio VARCHAR(255), -- Pode ser uma cidade ou distrito
    latitude DECIMAL(9, 6),
    longitude DECIMAL(9, 6),
    id_ativo INT NOT NULL,
    CONSTRAINT fk_ativo_localizacao FOREIGN KEY (id_ativo) REFERENCES Ativo(id_ativo)
);
```
