# 🏠 ArremateFocado — precificação de imóveis de leilão com Machine Learning

> Agregador de imóveis em leilão judicial e bancário da cidade de São Paulo, com estimativa de preço de mercado por IA para cada imóvel.
>
> **🔴 Em produção: [arrematefocado.com.br](https://arrematefocado.com.br)** — busca, mapa e calculadora de preço abertos, sem cadastro.

![Home do ArremateFocado](img/home-v2.png)

## O problema

Comprar imóvel em leilão em SP exige monitorar dezenas de sites de leiloeiros, destrinchar editais em PDF com endereços incompletos ("excelente apto na zona oeste") e descobrir sozinho se o valor de 1ª praça está barato — o "valor de avaliação" do laudo costuma estar defasado anos. Decisão de centenas de milhares de reais, sem referência de mercado.

## O que o sistema faz

**Coleta e organização**
- Monitoramento automatizado de **70+ fontes de leiloeiros** (Caixa incluída), com pipeline diário de extração, saneamento e estruturação dos dados
- **~2.400 lotes ativos** georreferenciados em mapa interativo com as **171 estações de metrô/trem** da cidade
- Documentos do leilão (edital, laudo, matrícula) organizados por imóvel

**Avaliação de Mercado por ML** — o coração do produto
- LightGBM treinado em **~277 mil transações reais** do ITBI da Prefeitura de São Paulo (dados públicos), com **janela flutuante de 4 anos**
- Cada imóvel recebe valor central + **banda de confiança de 70% (conformal prediction)** — estimativa honesta, não número mágico
- Métricas públicas na interface: **R² 0,81 · MAPE 22,4%**
- **Retreino mensal automatizado** com gate de regressão: se as métricas piorarem, o deploy é bloqueado

**Avaliação de Mercado** — valor central + banda de confiança de 70% por imóvel (card ampliado; [ver screenshot completo da página](img/avaliacao-mercado.webp)):

<img src="img/avaliacao-mercado-card.png" alt="Card de Avaliação de Mercado: valor central, banda de confiança e faixa mínimo/máximo" width="700">

**Produto completo**
- Assinatura (Stripe) com trial, calculadora pública de preço de mercado, autenticação, SEO técnico (sitemap dinâmico, JSON-LD, OG images automáticas)
- ~800 testes com CI (incluindo Postgres/PostGIS de serviço), observabilidade e backups diários offsite

## Arquitetura em uma frase

Pipeline diário automatizado — coleta multi-fonte → saneamento → enriquecimento (LLM para endereços em texto livre, OCR de documentos) → geocodificação em cascata → inferência de preço — servindo um Django com PostgreSQL/PostGIS e Redis, em Docker atrás de Cloudflare.

## Sobre este repositório

Este é o repositório de **vitrine técnica** do produto. O código-fonte é **privado** (produto comercial em operação). A demonstração funcional completa é o próprio site, aberto: **[arrematefocado.com.br](https://arrematefocado.com.br)**.

**Autor:** [Jean Carlo](https://www.linkedin.com/in/jean-carlo1993/) — projeto solo, do scraper ao deploy.
