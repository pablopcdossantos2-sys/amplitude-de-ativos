# Sino do Mercado

Webapp estático para visualizar **distribuições (“curvas de sino”)** de WINFUT, WDOFUT, DI1FUT e EWZ em diferentes janelas e timeframes.

## Acesse a versão online

GitHub Pages: https://pablopcdossantos2-sys.github.io/variacoes-graficos/

## O que o app faz

- importa CSVs OHLCV exportados do TradingView ou de outra fonte compatível;
- mantém um conjunto de dados separado para WINFUT, WDOFUT, DI1FUT e EWZ durante a sessão;
- permite analisar 20, 60, 120, 252 observações, todo o histórico ou um intervalo de datas;
- agrega candles para 5, 15, 30, 60 minutos ou diário, quando o arquivo possui granularidade suficiente;
- calcula retorno fechamento-fechamento, retorno logarítmico, retorno abertura-fechamento, amplitude percentual e variação absoluta;
- desenha histograma e curva normal ajustada;
- mostra média, desvio-padrão, mediana, assimetria, curtose, percentis e proporção de observações positivas/negativas;
- calcula a frequência histórica de um limiar e a probabilidade equivalente na normal ajustada;
- exporta o recorte usado na análise para CSV e o gráfico para PNG.

## Como usar

1. Abra o Webapp.
2. Escolha WINFUT, WDOFUT, DI1FUT ou EWZ.
3. Importe um CSV OHLCV.
4. Selecione a janela de análise ou um intervalo personalizado.
5. Escolha o timeframe e a métrica.
6. Analise o histograma, a curva normal e as estatísticas calculadas.

O parser procura colunas equivalentes a `time/date`, `open`, `high`, `low`, `close` e, opcionalmente, `volume`.

> A demonstração inicial usa dados **sintéticos**, apenas para apresentar o funcionamento da interface.

## Executar localmente

Como a aplicação está consolidada em um único `index.html`, basta abrir esse arquivo em um navegador moderno. O Plotly é carregado por CDN, portanto a geração dos gráficos requer acesso à internet.

Também é possível iniciar um servidor local:

```bash
python -m http.server 8000
```

Depois abra `http://localhost:8000`.

## GitHub Pages

O repositório inclui `.github/workflows/pages.yml`. Depois que o Pages for habilitado uma única vez no repositório, cada atualização da branch `main` será publicada automaticamente.

### Ativação inicial

No GitHub:

1. abra **Settings** do repositório;
2. entre em **Pages**;
3. em **Build and deployment**, defina **Source** como **GitHub Actions**;
4. salve a configuração;
5. abra **Actions → Deploy GitHub Pages** e execute novamente o workflow, ou faça um novo commit na `main`.

Depois dessa ativação inicial, a publicação passa a ser automática.

Endereço esperado: https://pablopcdossantos2-sys.github.io/variacoes-graficos/

## Dados da B3

O projeto não automatiza coleta do site da B3. Para WIN/WDO/DI, use uma fonte de dados que você tenha direito de utilizar e importe o CSV resultante. A camada de análise é independente da origem dos dados.

## Aviso

Ferramenta educacional. Distribuições históricas e curvas normais não garantem resultados futuros e não constituem recomendação de investimento.
