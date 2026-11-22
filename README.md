![Imagem](Cabecalho.png)

# Nanoesferas de sílica de Stöber dispersas em epóxi

Códigos usados no relatório de Iniciação à Pesquisa I (Ilum Escola de Ciência, CNPEM, 2026) para tratar os dados e gerar as figuras.

**Autoras:** Gabriela O. de Lima Cabral e Giovanna Avellar Machado

## Conteúdo

| Arquivo | Conteúdo |
| --- | --- |
| `XPS_ajuste_picos.ipynb` | Análise de XPS: espectro survey e ajuste dos picos C 1s, O 1s e Si 2p |
| `dados_xps/` | Espectros de XPS (arquivos `.avg`) lidos pelo notebook de XPS |
| `FTIR_RAMAN_MEV_CONDUÇÃOCALOR.ipynb` | Tratamento dos dados de Raman, FTIR e MEV e análise do ensaio de condução de calor |

### `XPS_ajuste_picos.ipynb`

Lê os arquivos `.avg` exportados pelo Avantage, ajusta cada região com picos Voigt e linha de base sigmoide usando a biblioteca `lmfit`, calcula a porcentagem de cada componente e monta a figura final com os quatro painéis. O ajuste segue a parte de deconvolução de picos do notebook *ATP-201 – Regressão não-linear*, do Prof. Daniel R. Cassar.

### `FTIR_RAMAN_MEV_CONDUÇÃOCALOR.ipynb`

- **Raman:** correção da linha de base (ALS), suavização (Savitzky–Golay) e posição das bandas da sílica.
- **FTIR:** espectro completo com as bandas identificadas e detalhe das regiões de O–H e da água.
- **Condução de calor:** diferença de temperatura (ΔT) entre as extremidades quente e fria das placas de epóxi e de epóxi + LAS, com média, desvio padrão e gráfico.
- **MEV:** histograma do diâmetro das partículas medidas no Fiji, com ajuste log-normal, e análise dos aglomerados.

## Como usar

1. Instale as bibliotecas:

   ```bash
   pip install numpy pandas scipy matplotlib lmfit jupyter
   ```

2. Abra o notebook no Jupyter e rode as células em ordem.

Os dados do XPS estão na pasta `dados_xps/`. O notebook de FTIR, Raman e MEV precisa dos arquivos `ramannp.txt` (Raman), `ftir_np_si.txt` (FTIR), `aglomerados.csv` e `aglomeradosnps.csv` (tabelas exportadas do Fiji) na mesma pasta dele. Os dados do ensaio de condução de calor já estão escritos no próprio notebook. Os gráficos são salvos em PNG na pasta do notebook.

## Licença

Distribuído sob a licença GPL-3.0. Veja o arquivo `LICENSE`.


