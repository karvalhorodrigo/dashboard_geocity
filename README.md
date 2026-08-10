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
2.  **Lotes por Usuário (Atualizações com Nomenclatura Unificada)**:
    *   Lista de ranking de produtividade baseada na coluna `usr_atualizado`.
    *   **postgres**: `13.634` lotes.
    *   **Não Informado**: `775` lotes (unificando `usuario_2_topogeo` e cadastros sem usuário).
    *   **Tiago**: `145` lotes (unificando `tiago_topogeo` e `TIAGO_003`).
    *   **Erivan Rocha**: `63` lotes (unificando `eriva_topogeo` e `ERIVAN_ROCHA_001`).
    *   **Rene Freitas**: `9` lotes (`RENE_FREITAS_002`).
    *   **Bruno**: `1` lote (`BRUNO_004`).
3.  **Lotes por Usuário por Dia (Linha do Tempo)**:
    *   Gráfico cronológico interativo (`chart-timeline`) baseado em `dt_atualizado` e `usr_atualizado`.
    *   Exclusão do usuário de sistema `postgres` para focar exclusivamente na produtividade da equipe em campo e escritório.
    *   Exclusão automática de usuários com 0 alterações da legenda e dos tooltips diários.

---

## 🛠️ Como Visualizar

O protótipo completo atualizado está salvo em seu workspace:

👉 **[Abrir Dashboard Atualizado (dashboard_gerencial.html)](file:///c:/Users/RodrigoC/Downloads/reuuubbb/dashboard_gerencial.html)**
