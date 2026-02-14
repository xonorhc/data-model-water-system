# LEVANTAMENTO DE REQUISITOS

## Sistema de Gestão de Ativos Geoespaciais – Abastecimento de Água

### Objetivo do Sistema

Desenvolver um **Sistema de Gestão de Ativos com suporte geoespacial** para o ciclo completo do abastecimento de água, permitindo:

- Cadastro estruturado e georreferenciado de ativos físicos
- Gestão do ciclo de vida dos ativos
- Controle técnico-operacional
- Gestão de manutenção preventiva e corretiva
- Integração com sistemas corporativos (ERP, Comercial, SCADA)
- Suporte a análises hidráulicas e combate a perdas
- Atendimento a exigências regulatórias e contábeis

**Público-alvo:**

- Engenharia
- Operação
- Manutenção
- Planejamento
- Controle de perdas
- Contabilidade/Patrimônio
- Regulação

**Abrangência:**
Do manancial até o ponto de entrega ao cliente (cavalete/hidrômetro).

---

### Diretrizes de Modelagem de Dados

#### Princípios Gerais

O modelo de dados deve:

- Ser estruturado em **Geodatabase**
- Utilizar separação clara entre:
  - Dados espaciais (geometria)
  - Dados alfanuméricos
  - Dados patrimoniais
  - Dados operacionais

- Implementar:
  - Domínios de atributos
  - Subtipos
  - Regras topológicas
  - Integridade referencial
  - Versionamento de edição
  - Auditoria de alterações

#### Tipos de Geometria

| Tipo de Ativo | Geometria  |
| ------------- | ---------- |
| Pontual       | Point      |
| Linear        | LineString |
| Área          | Polygon    |

---

### Estrutura Hierárquica de Ativos

#### Macroprocessos do Sistema

1. Produção
2. Tratamento
3. Adução
4. Reservação
5. Distribuição
6. Medição e Consumo

#### Estrutura Conceitual (Asset Hierarchy)

O modelo deve permitir hierarquia:

Sistema
→ Subsistema
→ Ativo Principal
→ Componente
→ Subcomponente

Exemplo:

Sistema de Distribuição
→ Zona de Pressão
→ Rede
→ Ramal Predial
→ Hidrômetro

---

### Escopo dos Ativos

Os ativos devem ser categorizados por tipo e função. Cada ativo precisa de atributos técnicos, operacionais e de localização.

Exemplo:

| Categoria                | Entidade (Feature Class)     | Atributos Principais (Exemplos)                                |
| ------------------------ | ---------------------------- | -------------------------------------------------------------- |
| Produção                 | Captação (Ponto)             | "Nome do manancial, vazão outorgada, tipo de bomba."           |
| Tratamento               | ETA (Polígono)               | "Capacidade nominal, tecnologia de tratamento, status."        |
| Transporte               | Adutoras (Linha)             | "Material, diâmetro nominal (DN), classe de pressão."          |
| Reservação               | Reservatórios (Polígono)     | "Tipo (apoiado, elevado, enterrado), capacidade, status."      |
| Distribuição             | Redes (Linha)                | "Ano de instalação, profundidade, método construtivo."         |
| Acessórios               | Válvulas/Registros (Ponto)   | "Tipo (gaveta/borboleta), estado (aberto/fechado)."            |
| Controle e Monitoramento | DMC (Polígono)               | "Zonas de pressão, macromedidores, sensores de pressão."       |
| Comercial                | Clientes/Hidrômetros (Ponto) | "Matrícula, categoria (residencial/comercial), consumo médio." |

---

### Modelo de Dados – Estrutura Recomendada

#### Entidade Base: ATIVO

Tabela genérica com atributos comuns:

- asset_id (PK)
- Código Corporativo
- Tipo
- Subtipo
- Sistema
- Data Implantação
- Status
- Vida Útil
- Valor Contábil
- Centro de Custo
- Município
- Setor Operacional
- Responsável Técnico

#### Atributos por Tipo de Ativo

##### Ativos Lineares

- Diâmetro Nominal
- Material
- Classe de Pressão
- Extensão (calculada automaticamente)
- Método Construtivo
- Ano de Instalação
- Revestimento

##### Ativos Eletromecânicos

- Potência
- Vazão Nominal
- Altura Manométrica
- Fabricante
- Modelo
- Número de Série (único)
- Tensão
- Fator de Serviço

##### Reservatórios

- Volume Nominal
- Volume Operacional
- Tipo Estrutural
- Cota de Fundo
- Cota de Extravasor

##### ETAs

- Capacidade Nominal
- Tipo de Tratamento
- Vazão Média Operacional
- Número de Linhas de Tratamento

##### Captações

- Tipo
- Vazão Outorgada
- Número da Outorga
- Corpo Hídrico

##### Clientes

- Matrícula
- Categoria
- Consumo Médio
- Situação da Ligação

---

### Requisitos Funcionais

#### RF01 – Cadastro de Ativos

Permitir cadastro, edição e inativação lógica de ativos.

#### RF02 – Georreferenciamento

Todos os ativos devem possuir geometria válida e sistema de coordenadas padronizado.

#### RF03 – Gestão de Manutenção

Permitir:

- Abertura de ordem de serviço
- Registro de manutenção preventiva/corretiva
- Controle de SLA
- Histórico completo por ativo

#### RF04 – Gestão Patrimonial

- Cálculo automático de depreciação (método linear)
- Integração com ERP
- Histórico contábil

#### RF05 – Análise Geoespacial

Permitir:

- Rastreamento upstream/downstream
- Identificação de áreas desabastecidas
- Análise por DMC
- Consulta por proximidade

#### RF06 – Visualização

- Mapa interativo
- Filtros por tipo/status
- Simbologia padronizada
- Visualização 2D e 3D

#### RF07 – Integração

Integração via API com:

- ERP
- Sistema Comercial
- SCADA
- Sistema de Qualidade da Água

---

### Regras de Negócio

RN01 – Todo ativo deve estar vinculado a um sistema e setor operacional.

RN02 – Exclusão física não permitida (apenas inativação lógica).

RN03 – Ativos lineares devem obedecer regras topológicas:

- Não permitir linhas soltas
- Não permitir sobreposição indevida
- Conectividade obrigatória em nós válidos

RN04 – Número de série único para ativos eletromecânicos.

RN05 – Domínios obrigatórios para atributos críticos (material, status, tipo).

RN06 – Todo hidrômetro deve estar vinculado a um ramal.

RN07 – Ramal deve estar conectado a uma rede.

RN08 – Alterações devem gerar log de auditoria.

RN09 – Sistema deve permitir versionamento de edição (ambiente multiusuário).

---

### Requisitos Não Funcionais

#### Arquitetura

- Banco geoespacial
- API REST
- Aplicação web responsiva
- Aplicativo móvel offline-first

#### Desempenho

- Tempo de resposta < 3s para consultas padrão
- Suporte a grandes volumes (> milhões de feições)

#### Segurança

- Controle de acesso por perfil (RBAC)
- Criptografia em trânsito (HTTPS)
- Backup automático
- Trilhas de auditoria

#### Geodésia

- Sistema de referência: SIRGAS 2000
- Projeção UTM conforme fuso da área de atuação
- Metadados obrigatórios para camadas

#### Mobilidade

- Sincronização offline
- Cache de mapas
- Geolocalização embarcada

---

### Governança de Dados

Recomenda-se incluir:

- Política de qualidade de dados
- Padronização de nomenclaturas
- Manual de simbologia
- Dicionário de dados corporativo
- Indicadores de qualidade (completude, consistência, conectividade)

---

### Exemplos de Casos de Uso

#### Caso de Uso 1: Cadastro de Estações de Tratamento

O usuário insere os dados da estação de tratamento, como localização (coordenadas geográficas), tipo de processo de tratamento, capacidade de operação e data de instalação.

#### Caso de Uso 2: Atualização de Estado de um Reservatório

Um técnico em campo atualiza o status de um reservatório (ex: "Em operação", "Em manutenção", "Vazio") diretamente via dispositivo móvel, com a sincronização imediata no sistema central.

#### Caso de Uso 3: Geração de Relatório de Perdas de Água

Um gestor solicita um relatório sobre a eficiência da rede de distribuição, que inclui a análise de perdas de água em diferentes áreas de uma cidade, com visualização geoespacial.

#### Caso de Uso 4: Migração de Dados Legados

O objetivo principal é a migração de dados legados (CAD/Planilhas) para um Geodatabase que suporte topologia de rede e análise de perdas.

---

### Tecnologias e Ferramentas Sugeridas

**Banco de Dados Geoespacial**: PostgreSQL/PostGIS, Oracle Spatial.

**Ferramentas de Visualização**: QGIS, ArcGIS, Google Maps API, OpenStreetMap.

**Plataforma de Integração**: RESTful APIs para integração com sistemas existentes (faturamento, controle de qualidade, etc.).

**Plataforma de Desenvolvimento**: Web (Angular, React) e Mobile (Flutter, React Native).

---
