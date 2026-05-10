# EpiWatch: Uma Aplicação Web Interativa para Simulação e Comparação de Doenças Infectocontagiosas Baseada no Modelo SEIR

**Autor: Cauã Vitor da Silva Araujo**
**Instituição: UFRPE — Bacharelado em Sistemas de Informação**
**Disciplina: Sustentabilidade em Sistemas de Informação**
**Professor: Jones Albuquerque**
**Data: Maio de 2026**
**IA utilizada: Claude (Anthropic) — claude.ai**

---

## Resumo

Este artigo descreve o desenvolvimento do EpiWatch, uma aplicação web interativa para simulação do espalhamento de doenças infectocontagiosas utilizando o modelo compartimental SEIR (Suscetíveis-Expostos-Infectados-Recuperados). A ferramenta, construída com HTML5, CSS3 e JavaScript puro, permite ao usuário ajustar parâmetros epidemiológicos em tempo real e observar imediatamente o efeito nas curvas de infecção. Um diferencial do EpiWatch é o módulo de comparação entre doenças reais (COVID-19, Influenza, Sarampo e Ebola), que executa quatro simulações paralelas e exibe os resultados em gráficos e tabela comparativa. O trabalho demonstra como ferramentas de inteligência artificial generativa permitem que estudantes de primeiro semestre desenvolvam aplicações funcionais e cientificamente fundamentadas.

**Palavras-chave:** Epidemiologia computacional; Modelo SEIR; Simulação de doenças; JavaScript; Saúde pública.

---

## Abstract

This paper describes the development of EpiWatch, an interactive web application for simulating the spread of infectious diseases using the SEIR (Susceptible-Exposed-Infected-Recovered) compartmental model. Built with HTML5, CSS3, and vanilla JavaScript, the tool allows users to adjust epidemiological parameters in real time and observe their effects on infection curves. A distinctive feature of EpiWatch is its disease comparison module, which runs four parallel simulations (COVID-19, Influenza, Measles, and Ebola) displaying results in synchronized charts and a comparative table. This work demonstrates how generative AI tools enable first-semester students to build functional and scientifically grounded applications.

**Keywords:** Computational epidemiology; SEIR model; Infectious disease simulation; JavaScript; Public health.

---

## 1. Introducao

A modelagem matematica de doencas infecciosas e um dos campos mais importantes da saude publica moderna. Durante a pandemia de COVID-19, expressoes antes restritas a especialistas como "achatar a curva", "R0" e "imunidade de rebanho" tornaram-se parte do vocabulario cotidiano. Por tras dessas expressoes estao modelos matematicos que tentam prever como uma doenca se espalha por uma populacao ao longo do tempo.

O modelo epidemiologico mais simples e ainda amplamente utilizado e o SIR, proposto por Kermack e McKendrick em 1927. Sua extensao natural, o modelo SEIR, adiciona um compartimento de individuos expostos (E), aqueles que foram infectados mas ainda estao no periodo de incubacao, sem transmitir a doenca. Essa distincao e fundamental para doencas como COVID-19 e sarampo, que possuem periodos de incubacao prolongados.

O presente trabalho tem dois objetivos: (1) implementar o modelo SEIR em uma aplicacao web interativa denominada EpiWatch; e (2) refletir sobre o processo de desenvolvimento assistido por inteligencia artificial, explorando tanto suas possibilidades quanto suas limitacoes pedagogicas.

---

## 2. Fundamentacao Teorica

### 2.1 O Modelo SEIR

O modelo SEIR divide uma populacao fechada de tamanho N em quatro compartimentos:

- S(t) — Suscetiveis: podem contrair a doenca
- E(t) — Expostos: infectados, mas no periodo de incubacao (nao transmitem)
- I(t) — Infectados: transmitem ativamente a doenca
- R(t) — Recuperados: imunes ou removidos

Com a restricao: S(t) + E(t) + I(t) + R(t) = N para todo t.

As equacoes diferenciais que governam o sistema sao:

```
dS/dt = - beta * S * I / N
dE/dt =   beta * S * I / N  -  sigma * E
dI/dt =   sigma * E  -  gamma * I
dR/dt =   gamma * I
```

| Parametro | Nome | Significado |
|---|---|---|
| beta | Taxa de transmissao | Contatos efetivos que geram infeccao por dia |
| sigma | Taxa de progressao | 1 / periodo de incubacao (dias) |
| gamma | Taxa de recuperacao | 1 / periodo infeccioso (dias) |

### 2.2 O Numero Basico de Reproducao (R0)

O numero basico de reproducao R0 representa quantos casos secundarios, em media, um unico infectado gera em uma populacao completamente susceptivel:

```
R0 = beta / gamma = beta * periodo infeccioso
```

- R0 > 1: a epidemia cresce
- R0 = 1: equilibrio endemico
- R0 < 1: a epidemia se extingue

Valores reais de R0: COVID-19 aprox. 2,5-3,5; Influenza aprox. 1,3-1,5; Sarampo aprox. 12-18; Ebola aprox. 1,5-2,5.

### 2.3 Camada Clinica

O EpiWatch incorpora uma camada clinica que deriva estimativas de hospitalizacoes e obitos como sistema paralelo ao motor de transmissao:

```
Hospitalizados(t) = I(t) * taxa_hospitalizacao
Obitos(t)        = R(t) * taxa_mortalidade
```

Essa abordagem garante que as taxas clinicas nao distorcam a dinamica de contagio.

---

## 3. Metodologia e Implementacao

### 3.1 Metodo Numerico

As equacoes diferenciais do SEIR sao resolvidas pelo Metodo de Euler com passo dt = 0,2 dias (5 iteracoes por dia):

```javascript
s += (-novaExposicao) * dt;
e += (novaExposicao - novaInfeccao) * dt;
i += (novaInfeccao  - recuperacao)  * dt;
r += recuperacao * dt;
```

Valores negativos sao truncados em zero para manter consistencia biologica do modelo.

### 3.2 Tecnologias Utilizadas

| Tecnologia | Papel no projeto |
|---|---|
| HTML5 | Estrutura da pagina (abas, sliders, canvas) |
| CSS3 | Design com variaveis CSS, grid layout, tema escuro |
| JavaScript Vanilla | Logica do modelo SEIR e atualizacao dos graficos |
| Chart.js (CDN) | Renderizacao dos graficos interativos |
| Google Fonts | Tipografia (Syne + Space Mono) |

A escolha por JavaScript puro garante que o arquivo index.html funcione diretamente no navegador, sem instalacao de dependencias.

### 3.3 Estrutura da Aplicacao

Modulo 1 - Simulador Livre: sliders para controlar beta, periodos de incubacao e infeccioso, tamanho da populacao, infectados iniciais, dias de simulacao, intervencao e taxas clinicas. Tres graficos atualizados em tempo real: curva SEIR completa, impacto clinico (hospitalizados e obitos) e comparacao do cenario com e sem intervencao. Cards com R0, pico de infectados, dia do pico e total de obitos.

Modulo 2 - Comparador de Doencas: quatro simulacoes paralelas (COVID-19, Influenza, Sarampo, Ebola) com parametros baseados na literatura cientifica. Cards de resumo por doenca, grafico comparativo de infectados simultaneos em 365 dias e tabela com metricas-chave.

### 3.4 Parametros dos Presets

| Doenca | beta | Incubacao | Infeccioso | R0 | Mortalidade |
|---|---|---|---|---|---|
| COVID-19 | 0,25 | 5 dias | 14 dias | 3,5 | 1,8% |
| Influenza | 0,50 | 2 dias | 3 dias | 1,5 | 0,1% |
| Sarampo | 1,00 | 10 dias | 7 dias | 7,0 | 0,2% |
| Ebola | 0,21 | 8 dias | 10 dias | 2,1 | 60% |

---

## 4. Resultados e Discussao

### 4.1 Curvas Epidemicas

Para uma populacao de 100.000 habitantes com 10 casos iniciais, os quatro modelos produzem curvas com caracteristicas distintas:

O Sarampo, com R0 = 7,0, gera o pico de infeccao mais rapido e de maior magnitude, podendo atingir mais de 80% da populacao. A Influenza apresenta curvas agudas e rapidas devido ao curto periodo infeccioso. O COVID-19 exibe curva mais alongada, reflexo do longo periodo de incubacao de 5 dias. O Ebola, apesar da mortalidade de 60%, apresenta curvas mais suaves porque sua taxa de transmissao beta e baixa. Esses resultados ilustram um principio central da epidemiologia: alta mortalidade nao implica alta transmissibilidade.

### 4.2 Efeito das Intervencoes

O terceiro grafico do Simulador Livre demonstra que reduzir beta em 50%, simulando uso de mascaras e distanciamento social, pode atrasar o pico em dezenas de dias e reduzir sua magnitude significativamente. Esse resultado ilustra visualmente o conceito de "achatar a curva", amplamente discutido durante a pandemia de COVID-19.

### 4.3 Limitacoes do Modelo

Os modelos compartimentais classicos assumem simplificacoes importantes: mistura homogenea da populacao, populacao fechada sem nascimentos ou migracao, imunidade permanente apos recuperacao, e parametros constantes ao longo do tempo. Modelos mais sofisticados como os baseados em agentes ou em redes de contato superam essas limitacoes, mas exigem substancialmente maior complexidade computacional.

---

## 5. Conclusao

O EpiWatch demonstra que o modelo SEIR, apesar de suas simplificacoes, e uma ferramenta poderosa para compreender os mecanismos fundamentais do espalhamento de doencas infecciosas. A implementacao em JavaScript puro garante acessibilidade maxima: qualquer pessoa com um navegador pode executar a simulacao sem instalar nada.

Do ponto de vista pedagogico, este trabalho ilustra com precisao o paradoxo levantado pelo professor: com auxilio de inteligencia artificial generativa, um estudante de primeiro semestre foi capaz de construir uma aplicacao funcionalmente correta e visualmente sofisticada. No entanto, isso nao significa que o estudante compreenda as equacoes diferenciais que sustentam o modelo, a derivacao matematica de R0, ou os fundamentos numericos do Metodo de Euler. O codigo funciona; entende-lo profundamente e o verdadeiro desafio, e o verdadeiro objetivo de uma formacao universitaria em Sistemas de Informacao.

---

## Referencias

- KERMACK, W. O.; McKENDRICK, A. G. A contribution to the mathematical theory of epidemics. Proceedings of the Royal Society of London. Series A, v. 115, n. 772, p. 700-721, 1927.
- ANDERSON, R. M.; MAY, R. M. Infectious Diseases of Humans: Dynamics and Control. Oxford: Oxford University Press, 1991.
- HETHCOTE, H. W. The mathematics of infectious diseases. SIAM Review, v. 42, n. 4, p. 599-653, 2000.
- IRRD. Calculadora Epidemica. Disponivel em: https://www.irrd.org/covid-19/calculadora-epidemica/. Acesso em: maio 2026.

---

## Apendice — Prompts Utilizados

### Prompt para o codigo (EpiWatch)

IA utilizada: Claude (Anthropic) — claude.ai

"Voce e um desenvolvedor web senior. Crie uma aplicacao web de pagina unica chamada EpiWatch para simular o espalhamento de doencas infectocontagiosas usando o modelo SEIR. A aplicacao deve ter duas abas: (1) Simulador Livre com sliders para controlar beta, periodo de incubacao, periodo infeccioso, tamanho da populacao, infectados iniciais, dias de simulacao, efeito de intervencao e taxas clinicas; deve exibir cards com metricas (R0, pico, dia do pico, obitos) e tres graficos: curva SEIR, impacto clinico e comparacao com/sem intervencao; e (2) Comparador de Doencas que executa quatro simulacoes paralelas para COVID-19, Influenza, Sarampo e Ebola com parametros reais da literatura, exibindo cards resumo, grafico comparativo de infectados e tabela com metricas. Use HTML, CSS e JavaScript puros com Chart.js via CDN. O design deve ser moderno, escuro e profissional com tipografia diferenciada. Use o Metodo de Euler com dt=0,2. A aplicacao deve funcionar apenas abrindo o index.html em um navegador."

### Prompt para o artigo cientifico

IA utilizada: Claude (Anthropic) — claude.ai

"Escreva um artigo cientifico em portugues explicando o projeto EpiWatch, uma aplicacao web interativa que simula doencas infectocontagiosas usando o modelo SEIR implementado em JavaScript puro. O artigo deve conter: resumo com palavras-chave e abstract em ingles, introducao contextualizando a modelagem epidemiologica, fundamentacao teorica com as equacoes SEIR e definicao de R0 com exemplos de doencas reais, metodologia com Metodo de Euler, tecnologias usadas e parametros dos presets, resultados e discussao analisando as curvas e o efeito de intervencoes, conclusao refletindo sobre o paradoxo pedagogico do desenvolvimento assistido por IA, referencias e apendice com os prompts. Autor: Caua Vitor da Silva Araujo, Sistemas de Informacao."
