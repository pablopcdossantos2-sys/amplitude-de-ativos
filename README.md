# Sino do Mercado

Webapp estático para estudar, por meio de **histogramas e curva de sino**, a distribuição histórica das **amplitudes (Máxima − Mínima)** de WINFUT, WDOFUT, DI1FUT e EWZ.

## Acesse a versão online

GitHub Pages: https://pablopcdossantos2-sys.github.io/variacoes-graficos/

## Objetivo da ferramenta

A pergunta central do projeto é:

> **Qual é a distribuição das amplitudes que um ativo costuma percorrer dentro de uma determinada faixa de tempo?**

A amplitude de uma observação é calculada como:

`Amplitude = maior High − menor Low`

O Webapp oferece dois modos de análise.

### 1. Faixa horária por pregão

Este é o modo principal.

Exemplo: selecionar **09:00–10:00** e analisar os últimos **60 pregões**.

Para cada pregão, o sistema:

1. seleciona todos os candles compreendidos entre 09:00 e 10:00;
2. encontra a maior máxima da faixa;
3. encontra a menor mínima da faixa;
4. calcula `maior máxima − menor mínima`;
5. usa esse resultado como uma observação no histograma.

Assim, se houver 60 pregões válidos, haverá até 60 amplitudes compondo a distribuição.

### 2. Cada candle / timeframe

Neste modo, cada candle é uma observação independente.

É possível estudar as amplitudes dos candles no timeframe original do CSV ou reagrupar dados suficientemente granulares em 2, 5, 10, 15, 30 e 60 minutos, além do diário.

## Formas de medir a amplitude

### Amplitude absoluta

`High − Low`

Útil para responder perguntas como “quantos pontos o WIN costuma percorrer nesta faixa?”.

### Amplitude percentual

`((High − Low) / Open) × 100`

Útil para comparar períodos ou ativos em escalas de preço diferentes.

## Comparador de faixas horárias

O Webapp possui um painel para comparar até quatro intervalos do pregão simultaneamente. Os valores padrão são:

- 09:00–09:30;
- 09:30–10:00;
- 10:00–11:00;
- 11:00–12:00.

Cada faixa é configurável. O painel desenha curvas de densidade empírica suavizada (KDE) e apresenta, lado a lado, amostra, média, mediana, P75, P95 e maior amplitude. Assim é possível visualizar em quais horários o ativo historicamente tende a expandir mais.

## Indicador TradingView — Contexto Estatístico 21D

O projeto inclui um indicador em Pine Script v6 que abre em uma **janela independente do gráfico de preço**.

A base continua sendo a comparação entre **preço atual** e **SMA dos últimos 21 pregões**, mas a versão atual acrescenta uma camada de contexto estatístico.

### Recursos implementados

- percentil atual do afastamento do preço para a média;
- distância normalizada em ATR diário;
- persistência na zona atual e persistência diária em extremos;
- estatísticas separadas para afastamentos acima e abaixo da média;
- regime de volatilidade (baixa, normal ou alta);
- opção de comparar o preço apenas com observações históricas do mesmo regime de volatilidade;
- direção da SMA (ascendente, lateral ou descendente);
- estudo dos eventos históricos de entrada em P95/P5;
- retorno mediano em direção à média após 1, 3 e 5 pregões;
- porcentagem de eventos que retornaram à faixa central em 1, 3 e 5 pregões;
- mediana e P75 do tempo necessário para voltar à faixa central;
- mediana e P75 da excursão adicional depois da entrada em extremo;
- painel comparativo normalizado de WIN, WDO, DI e EWZ;
- alertas seletivos com opção de exigir fechamento do candle.

### Faixas estatísticas

O usuário pode escolher:

- **Percentis (padrão):** P25–P75 como faixa central e P5–P95 como faixa externa;
- **Desvio-padrão:** média histórica da distância ±1σ e ±2σ.

O indicador não interpreta P95/P5 como ordens automáticas de venda/compra. Os extremos são registrados como **eventos de pesquisa**, permitindo medir tanto reversão quanto continuidade.

### Comparação entre ativos

Os símbolos padrão são:

- `BMFBOVESPA:WIN1!`;
- `BMFBOVESPA:WDO1!`;
- `BMFBOVESPA:DI11!`;
- `AMEX:EWZ`.

Todos podem ser alterados nas configurações do indicador.

Página explicativa: https://pablopcdossantos2-sys.github.io/variacoes-graficos/indicador-media-21d.html

Código Pine: `tradingview-preco-media-21d.pine`

> O Pine Script não importa os CSVs locais do Webapp. As análises históricas de amplitude continuam sendo feitas pelo Webapp; o indicador usa os dados disponíveis no TradingView para o símbolo aberto e para os símbolos do painel comparativo.


## Integração com o Contexto de Mercado

O **Sino do Mercado** ajuda a investigar como as amplitudes históricas se distribuem; o projeto [Contexto de Mercado](https://pablopcsantos.github.io/contexto-de-mercado/) ajuda a transformar observações em preparação, cenários condicionais e revisão do processo. As ferramentas são complementares e não devem ser confundidas com geradores de sinais.

### Fluxo de trabalho sugerido

1. Escolha o ativo, o recorte horário ou timeframe e a janela histórica.
2. Verifique a granularidade do CSV e o tamanho da amostra antes de interpretar os resultados.
3. Registre mediana, P75 e P95 como descrições da distribuição histórica, sem convertê-los em previsões de reversão.
4. Na preparação pré-mercado, escreva cenários condicionais, gatilhos observáveis, invalidações e condições para não operar.
5. Depois do pregão, compare a hipótese com o que ocorreu e revise o processo, sem avaliar a qualidade da leitura apenas pelo resultado financeiro.

A integração atual é por navegação e metodologia: os CSVs e os dados locais de cada aplicação não são transferidos automaticamente entre projetos.

## Estatísticas apresentadas

O Webapp calcula média, mediana, desvio-padrão, assimetria, curtose excedente, percentis 5%, 25%, 75% e 95%, menor e maior amplitude, tamanho da amostra e frequência histórica acima ou abaixo de um limiar escolhido.

A curva normal ajustada é mostrada apenas como **referência visual**. Amplitudes são não negativas e não precisam seguir distribuição normal; o histograma dos dados observados é a informação principal.

## Dados reais

O Webapp importa CSVs OHLCV exportados do TradingView, Profit/Nelogica ou outra fonte compatível. O parser procura colunas equivalentes a `time/date`, `open`, `high`, `low`, `close` e `volume` (opcional). Os arquivos são processados no próprio navegador.

É possível escolher entre **Substituir os dados deste ativo** e **Adicionar/mesclar ao histórico**. No modo de mesclagem, vários CSVs de períodos diferentes são combinados, ordenados por timestamp e deduplicados automaticamente.

Tutorial completo: https://pablopcdossantos2-sys.github.io/variacoes-graficos/tutorial-dados.html

> A demonstração incluída no Webapp usa dados sintéticos intradiários apenas para explicar a interface.

## Granularidade do CSV

Para uma análise correta, o arquivo precisa ter resolução suficiente. Por exemplo, para calcular com precisão uma faixa de **09:00–09:15**, use candles menores ou iguais a essa faixa, idealmente de 1 ou 5 minutos. O Webapp consegue agregar candles para períodos maiores, mas não consegue reconstruir detalhes inexistentes no arquivo original.

## Executar localmente

A aplicação está consolidada em um único `index.html`. Basta abrir o arquivo em um navegador moderno. O Plotly é carregado por CDN.

Também é possível iniciar um servidor local:

```bash
python -m http.server 8000
```

Depois abra `http://localhost:8000`.

## GitHub Pages

O repositório inclui `.github/workflows/pages.yml`. Em **Settings → Pages**, configure **Source** como **GitHub Actions**. Depois disso, commits na branch `main` publicam automaticamente a nova versão.

## Fontes sugeridas

- **TradingView:** exporta para CSV os dados carregados no gráfico e é uma rota prática para WIN, WDO, DI e EWZ;
- **Profit/Nelogica:** possui exportação de dados históricos OHLCV para ativos da B3;
- **Base Histórica Nelogica:** alternativa comercial para históricos intradiários mais extensos;
- **B3:** fonte oficial útil para histórico, boletins, ajustes e resumos, embora a área pública não seja uma fonte direta de candles intradiários de 1/5 minuto para este projeto.

O projeto não automatiza coleta do site da B3. Utilize apenas dados aos quais você tenha direito de acesso e uso.

## Aviso

Ferramenta educacional para análise histórica. Os resultados não constituem recomendação de investimento e não garantem comportamento futuro.
