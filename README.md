# Dashboard Gerencial - Protótipo SIG & Atividade Cadastral (Acaraú/CE)

Atualizamos o protótipo do dashboard gerencial para incluir os novos indicadores solicitados e fornecer recomendações de **WebGIS profissional** para representação das suas camadas poligonais.

---

## 🗺️ Alternativas de Mapas e WebGIS para Camadas Poligonais

Como o banco possui dados espaciais robustos (e views prontas gerando GeoJSON como `api_publica.v_site_lote_geojson`), você pode evoluir o mapa básico para um **WebGIS profissional** utilizando as seguintes ferramentas:

### 1. MapLibre GL JS / Mapbox GL (Implementado no Protótipo)
*   **Hospedagem Estática de Alta Performance**: O mapa agora é renderizado usando **MapLibre GL JS** com aceleração via WebGL no navegador.
*   **Dados Reais do Banco (Polígonos)**: Consulta e transformação via PostGIS de **400 lotes do Centro** (`ST_Transform(geom, 4326)` do SRID UTM `31984` para WGS84 `4326`), incorporados diretamente como fonte GeoJSON dinâmica.
*   **Estilização Profissional**: Os lotes são exibidos em **verde translúcido com bordas laranjas**, idênticos à referência visual que você enviou.
*   **Fundo de Satélite**: Usamos o mapa base de satélite de alta resolução **Esri World Imagery** para compor o plano de fundo.
*   **Interatividade**: Hover animado que destaca o contorno do lote sob o cursor do mouse e popup ao clicar exibindo o bairro, código BCI, nome do proprietário e o status do cadastro em tempo real.

### 2. OpenLayers (Ideal para SIG Técnico / Engenharia)
*   **Por que usar**: É a biblioteca mais tradicional e completa de SIG para a web. Suporta projeções locais complexas de forma nativa (ex: **SIRGAS 2000 / UTM zone 24S** - EPSG:31984, muito comum no Ceará), o que evita distorções de cálculo de área.
*   **Recursos**: Integração direta com GeoServer ou QGIS Server (serviços WMS, WFS e WFS-T para edição online).

### 3. Deck.gl (Ideal para Big Data e Mapas Analíticos)
*   **Por que usar**: Projetado pelo Uber para visualização de grandes volumes de dados. Permite criar mapas hexagonais de densidade de cadastro, grids de infraestrutura e animações cronológicas de lotes criados ao longo do tempo.

---

## 📊 Novos Indicadores e Gráficos Adicionados

O arquivo [`dashboard_gerencial.html`](file:///c:/Users/RodrigoC/Downloads/reuuubbb/dashboard_gerencial.html) foi atualizado com dados reais extraídos diretamente das suas tabelas no Supabase:

1.  **Lotes com Cadastro vs. Sem Cadastro**:
    *   Substituiu o antigo gráfico de uso.
    *   **Métrica**: Definimos como **"Com Cadastro"** os lotes que possuem o nome do proprietário (`nm_proprietario_espelho`) preenchido no banco de dados.
    *   **Resultado**: **10.216 lotes** (69.85%) estão cadastrados e **4.409 lotes** (30.15%) constam sem cadastro.
2.  **Lotes Adicionados por Usuário**:
    *   Lista de ranking de usuários baseada na coluna `usr_criado`.
    *   **postgres**: `14.566` lotes cadastrados.
    *   **tiago_topogeo**: `6` lotes cadastrados.
    *   **eriva_topogeo**: `1` lote cadastrado.
    *   **TIAGO_003**: `1` lote cadastrado.
    *   *Sem informação de usuário*: `51` lotes.
3.  **Lotes Adicionados por Usuário por Dia**:
    *   Gráfico de linha cronológico interativo (`chart-timeline`).
    *   Exibe a contagem de lotes criados dia a dia para cada usuário ativo no banco (mostrando picos de cadastro no final de Julho por `tiago_topogeo` e cadastros em Agosto por `TIAGO_003`).

---

## 🛠️ Como Visualizar

O protótipo completo atualizado está salvo em seu workspace:

👉 **[Abrir Dashboard Atualizado (dashboard_gerencial.html)](file:///c:/Users/RodrigoC/Downloads/reuuubbb/dashboard_gerencial.html)**
