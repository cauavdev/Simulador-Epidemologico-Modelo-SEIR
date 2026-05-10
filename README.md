# 🦠 EpiWatch — Simulador de Doenças Infectocontagiosas

> **Cauã Vitor da Silva Araujo** · Bacharelado em Sistemas de Informação  
> Disciplina: Sustentabilidade em Sistemas de Informação · Professor: Jones Albuquerque  
> IA utilizada: Claude (Anthropic) — claude.ai

---

## Sobre o Projeto

O **EpiWatch** é uma aplicação web interativa para simulação e comparação do espalhamento de doenças infectocontagiosas, baseada no modelo compartimental **SEIR** (Suscetíveis → Expostos → Infectados → Recuperados).

O diferencial em relação a outras implementações é o **módulo de comparação entre doenças reais**: COVID-19, Influenza, Sarampo e Ebola são simulados em paralelo com parâmetros baseados na literatura científica, permitindo visualizar lado a lado como cada doença se comporta em uma mesma população.

---

## Funcionalidades

**Aba 1 — Simulador Livre**
- Sliders interativos para controlar todos os parâmetros (β, σ, γ, população, intervenção, taxas clínicas)
- 4 cards de métricas: R₀, pico de infectados, dia do pico e total de óbitos
- Gráfico 1: Curva epidêmica SEIR completa
- Gráfico 2: Impacto no sistema de saúde (hospitalizados e óbitos acumulados)
- Gráfico 3: Comparação do cenário com e sem intervenção (efeito de quarentena/máscaras)
- Presets rápidos: COVID-19, Gripe, Sarampo, Ebola ou personalizado

**Aba 2 — Comparador de Doenças**
- Simulação paralela das 4 doenças com parâmetros reais
- Cards de resumo por doença
- Gráfico comparativo de infectados simultâneos ao longo de 365 dias
- Tabela com R₀, pico, dia do pico e óbitos estimados

---

## Como Executar

Não é necessário instalar nada. Basta:

1. Baixar ou clonar este repositório
2. Abrir o arquivo `index.html` em qualquer navegador moderno (Chrome, Firefox, Edge)

```bash
git clone https://github.com/cauavdev/epiwatch.git
cd epiwatch
# Abrir index.html no navegador
```

---

## Tecnologias

| Tecnologia | Função |
|---|---|
| HTML5 | Estrutura da página |
| CSS3 | Design (tema escuro, grid, variáveis CSS) |
| JavaScript Vanilla | Modelo SEIR + Método de Euler |
| Chart.js | Gráficos interativos (via CDN) |
| Google Fonts | Tipografia (Syne + Space Mono) |

---

## O Modelo Matemático (SEIR)

```
dS/dt = - β · S · I / N
dE/dt =   β · S · I / N  -  σ · E
dI/dt =   σ · E  -  γ · I
dR/dt =   γ · I

R₀ = β / γ
```

- **β (beta)**: taxa de transmissão
- **σ (sigma)**: 1 / período de incubação
- **γ (gamma)**: 1 / período infeccioso
- **R₀**: número básico de reprodução — se > 1, a epidemia cresce

---

## Arquivos

| Arquivo | Descrição |
|---|---|
| `index.html` | Aplicação completa (HTML + CSS + JS em um único arquivo) |
| `artigo_cientifico.md` | Artigo acadêmico explicando o projeto |
| `README.md` | Este arquivo |

---

## Prompts Utilizados

### Código (EpiWatch)
**IA:** Claude (Anthropic) — claude.ai

> *"Você é um desenvolvedor web sênior. Crie uma aplicação web de página única chamada EpiWatch para simular o espalhamento de doenças infectocontagiosas usando o modelo SEIR. A aplicação deve ter duas abas: (1) Simulador Livre com sliders para controlar beta, período de incubação, período infeccioso, tamanho da população, infectados iniciais, dias de simulação, efeito de intervenção e taxas clínicas; deve exibir cards com métricas (R₀, pico, dia do pico, óbitos) e três gráficos: curva SEIR, impacto clínico e comparação com/sem intervenção; e (2) Comparador de Doenças que executa quatro simulações paralelas para COVID-19, Influenza, Sarampo e Ebola com parâmetros reais da literatura. Use HTML, CSS e JavaScript puros com Chart.js via CDN. Design moderno, escuro e profissional. Método de Euler com dt=0,2."*

### Artigo Científico
**IA:** Claude (Anthropic) — claude.ai

> *"Escreva um artigo científico em português explicando o projeto EpiWatch, uma aplicação web interativa que simula doenças infectocontagiosas usando o modelo SEIR em JavaScript puro. Inclua: resumo e abstract, introdução, fundamentação teórica com equações e R₀, metodologia, resultados e discussão, conclusão refletindo sobre o paradoxo pedagógico do desenvolvimento assistido por IA, referências e apêndice com os prompts. Autor: Cauã Vitor da Silva Araujo, Sistemas de Informação."*

---

*Projeto acadêmico desenvolvido com auxílio de IA, conforme orientação do professor.*
