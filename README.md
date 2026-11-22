![Imagem](Cabecalho.png)

# Nanoesferas de sílica de Stöber dispersas em epóxi

![Python](https://img.shields.io/badge/Python-3.13-3776AB?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

Códigos usados no relatório de Iniciação à Pesquisa I (Ilum Escola de Ciência, CNPEM, 2026) para tratar os dados de caracterização e gerar as figuras.

**Autoras:** Gabriela O. de Lima Cabral e Giovanna Avellar Machado

## Sobre o projeto

O projeto investigou se nanopartículas de sílica de Stöber, dispersas em resina epóxi sem silano pela rota LAS de Tanahashi e Hirota (2023), melhoram a condução de calor do material, pensando no encapsulamento de chips. A hipótese era que os grupos silanol (Si–OH) da superfície da sílica formariam ligações de hidrogênio com a matriz e reduziriam a resistência térmica da interface.

Este repositório reúne o tratamento dos dados das caracterizações das nanopartículas (MEV, Raman, FTIR e XPS) e do ensaio de condução de calor do compósito. As medidas de DLS e o ensaio de toxicidade com *Daphnia similis*, também descritos no relatório, não fazem parte deste repositório.

## Estrutura do repositório

```text
IP-1/
├── README.md
├── Cabecalho.png
├── XPS_ajuste_picos.ipynb                # análise de XPS
├── FTIR_RAMAN_MEV_CONDUÇÃOCALOR.ipynb    # Raman, FTIR, condução de calor e MEV
├── dados_xps/
│   ├── XPS Survey.avg
│   ├── C1s Scan.avg
│   ├── O1s Scan.avg
│   └── Si2p Scan.avg
└── dados_FTIR_RAMAN_MEV/
    ├── ramannp.txt
    ├── ftir_np_si.txt
    ├── aglomerados.csv
    └── aglomeradosnps.csv
```

## Dados

| Arquivo | Técnica e equipamento | Conteúdo |
| --- | --- | --- |
| `dados_xps/*.avg` | XPS, K-Alpha (Thermo Fisher Scientific), fonte Al Kα | Survey e espectros de alta resolução de C 1s, O 1s e Si 2p, exportados pelo Avantage |
| `ramannp.txt` | Raman, MacroRAM (Horiba), laser de 785 nm | Deslocamento Raman (cm⁻¹) e intensidade (contagens) |
| `ftir_np_si.txt` | FTIR-ATR, IRSpirit com acessório QATR-S (Shimadzu) | Número de onda (cm⁻¹) e transmitância (%) |
| `aglomerados.csv` | MEV, Helios NanoLab 600 (Thermo Fisher Scientific, LNNano) | Tabela *Results* do Fiji com o diâmetro de cada partícula medida |
| `aglomeradosnps.csv` | MEV | Tabela do *Analyze Particles* do Fiji com a área e a circularidade dos aglomerados |

Os dados do ensaio de condução de calor (temperaturas nas extremidades quente e fria de cada placa) estão escritos diretamente no notebook.

## Análises

### `XPS_ajuste_picos.ipynb`

O notebook lê os arquivos `.avg` e converte a energia cinética em energia de ligação a partir da energia do fóton registrada no próprio arquivo. O survey é apenas plotado, com a identificação dos picos de fotoelétrons (O 1s, C 1s, F 1s, Si 2p, Si 2s, O 2s e N 1s) e dos picos Auger (O KLL, F KLL e C KLL). As regiões de alta resolução são ajustadas com a biblioteca `lmfit`, usando picos Voigt e uma linha de base formada por uma constante e uma sigmoide para cada pico, pelo método `least_squares`.

| Região | Componentes ajustadas |
| --- | --- |
| C 1s | C–C/C–H (referência em 284,8 eV), C–O, O–C=O e CF₂ |
| O 1s | Si–O (SiO₂) |
| Si 2p | Si⁴⁺ (SiO₂) |

Para cada região, o notebook imprime o centro, a largura a meia altura (FWHM) e a porcentagem de área de cada componente e, no final, monta uma figura com os quatro painéis. O ajuste segue a parte de deconvolução de picos do notebook *ATP-201 – Regressão não-linear*, do Prof. Daniel R. Cassar, com as mudanças descritas no início do notebook. A linha de base usada não é a de Shirley, que é a mais comum na literatura de XPS.

### `FTIR_RAMAN_MEV_CONDUÇÃOCALOR.ipynb`

**Raman.** O espectro é recortado entre 300 e 3450 cm⁻¹, porque abaixo de cerca de 300 cm⁻¹ está o corte do filtro do laser. A linha de base é corrigida por mínimos quadrados assimétricos (ALS, com λ = 10⁶ e p = 0,01) e o espectro é suavizado pelo filtro de Savitzky–Golay (janela de 31 pontos e polinômio de grau 3). Em seguida, o notebook localiza as bandas da rede Si–O–Si, a banda D2 dos anéis de três membros e a banda de O–H.

**FTIR.** O espectro é usado entre 400 e 4000 cm⁻¹. Uma versão suavizada por Savitzky–Golay (janela de 15 pontos e grau 3) serve para localizar cada banda, tomada como o mínimo de transmitância perto do valor esperado. O notebook gera o espectro completo e um detalhe das regiões de O–H (3900 a 2600 cm⁻¹) e da água adsorvida (1900 a 1450 cm⁻¹).

**Condução de calor.** Para cada placa (epóxi e epóxi + LAS), o notebook calcula a diferença de temperatura ΔT = T<sub>quente</sub> − T<sub>fria</sub> nos pares de pontos 1–4, 2–5 e 3–6, com a média e o desvio padrão amostral, e faz um gráfico de barras com os pontos individuais.

**MEV.** O notebook lê a tabela de diâmetros do Fiji, corrige a escala e remove as medidas duplicadas. A correção é necessária porque a calibração usada no Fiji fez a barra de 500 nm medir 3375 unidades, então todas as medidas são multiplicadas por 500/3375. Com as 800 partículas, calcula a média, o desvio padrão, a mediana e o coeficiente de variação, ajusta uma distribuição log-normal e compara as três séries de medida pelo teste *t* de Welch. Também estima a área superficial específica, supondo esferas lisas de densidade 2,0 g/cm³, e salva os diâmetros corrigidos em `diametros_corrigidos_nm.csv`. Na análise dos aglomerados, converte a área de cada objeto em número equivalente de partículas de 19 nm, calcula a fração da imagem coberta e avalia a relação entre área e circularidade pela correlação de Spearman.

## Figuras e arquivos gerados

Os arquivos são salvos na pasta principal do repositório, e as figuras em PNG com 300 dpi. Os gráficos de Raman e FTIR saem na versão clara (`_claro`), usada no relatório. Para gerar a versão escura (`_escuro`), com fundo transparente para slides, mude `FUNDO_ESCURO = True` na célula de configurações. Os gráficos de condução de calor e de MEV saem nas duas versões.

| Arquivo | Conteúdo |
| --- | --- |
| `xps_figura.png` | Painel com o survey e os ajustes de C 1s, O 1s e Si 2p |
| `raman_silica_bruto_claro.png` | Espectro Raman bruto |
| `raman_silica_linha_base_claro.png` | Espectro Raman com a linha de base ALS |
| `raman_silica_rede_claro.png` | Região da rede Si–O–Si (300 a 1250 cm⁻¹) |
| `raman_silica_completo_claro.png` | Espectro Raman completo tratado |
| `ftir_silica_claro.png` | Espectro FTIR completo com as bandas identificadas |
| `ftir_silica_OH_claro.png` | Detalhe das regiões de O–H e da água |
| `deltaT_placas_claro.png` e `_escuro.png` | ΔT de cada par de pontos nas duas placas |
| `histograma_mev_claro.png` e `_escuro.png` | Histograma dos diâmetros com o ajuste log-normal |
| `aglomerados_mev_claro.png` e `_escuro.png` | Circularidade em função do tamanho dos aglomerados |
| `painel_mev_claro.png` e `_escuro.png` | Painel com (a) a distribuição de diâmetros e (b) os aglomerados |
| `diametros_corrigidos_nm.csv` | Diâmetros das partículas após a correção de escala |

## Principais resultados

| Técnica | Resultado |
| --- | --- |
| MEV | Diâmetro de 19,0 ± 6,2 nm (n = 800; mediana de 18,2 nm); os aglomerados cobrem 37% da imagem, e o maior equivale a cerca de 530 partículas |
| Raman | Bandas da rede Si–O–Si em 439, 798 e 1048 cm⁻¹, banda D2 em 601 cm⁻¹ e O–H em 3176 cm⁻¹ |
| FTIR | Si–O–Si em 1079, 797 e 451 cm⁻¹, Si–OH em 961 cm⁻¹ e água adsorvida (H–O–H) em 1632 cm⁻¹ |
| XPS | O 1s em 532,56 eV e Si 2p em 103,33 eV, compatíveis com SiO₂; C 1s com quatro componentes, incluindo 1% de CF₂ |
| Condução de calor | ΔT de 3,8 ± 2,1 °C na epóxi e de 4,5 ± 2,5 °C na epóxi + LAS |

## Como reproduzir

1. Clone o repositório:

   ```bash
   git clone https://github.com/gabikbrown/IP-1.git
   cd IP-1
   ```

2. Instale as bibliotecas:

   ```bash
   pip install numpy pandas scipy matplotlib lmfit jupyter
   ```

3. Abra o Jupyter a partir da pasta do repositório:

   ```bash
   jupyter notebook
   ```

4. Abra um dos notebooks e rode todas as células em ordem (menu *Run* → *Run All Cells*).

Os notebooks procuram os dados nas pastas `dados_xps/` e `dados_FTIR_RAMAN_MEV/` a partir da pasta principal, por isso o Jupyter precisa ser aberto nela. Eles foram testados com Python 3.13, NumPy 2.5, SciPy 1.18, Matplotlib 3.11, pandas 3.0 e lmfit 1.3. Se o Jupyter avisar que não encontrou o kernel do notebook de XPS (`ilumpy`), escolha o kernel Python 3.

## Referências

- CASSAR, D. R. *ATP-201: regressão não-linear* (notebook de aula). Ilum Escola de Ciência, CNPEM, 2026.
- EILERS, P. H. C.; BOELENS, H. F. M. *Baseline correction with asymmetric least squares smoothing*. Leiden: Leiden University Medical Centre, 2005.
- NEWVILLE, M.; STENSITZKI, T.; ALLEN, D. B.; INGARGIOLA, A. *LMFIT: Non-linear least-square minimization and curve-fitting for Python*. Zenodo, 2014. DOI: [10.5281/zenodo.11813](https://doi.org/10.5281/zenodo.11813).
- SAVITZKY, A.; GOLAY, M. J. E. Smoothing and differentiation of data by simplified least squares procedures. *Analytical Chemistry*, v. 36, n. 8, p. 1627–1639, 1964.
- SCHINDELIN, J. et al. Fiji: an open-source platform for biological-image analysis. *Nature Methods*, v. 9, n. 7, p. 676–682, 2012.
- STÖBER, W.; FINK, A.; BOHN, E. Controlled growth of monodisperse silica spheres in the micron size range. *Journal of Colloid and Interface Science*, v. 26, n. 1, p. 62–69, 1968. DOI: [10.1016/0021-9797(68)90272-5](https://doi.org/10.1016/0021-9797(68)90272-5).
- TANAHASHI, M.; HIROTA, K. A silica/epoxy resin nanocomposite exhibiting high thermal stability and low thermal expansion based on the uniform dispersion of hydrophilic colloidal silica nanospheres. *Polymers and Polymer Composites*, v. 31, 2023. DOI: [10.1177/09673911231171479](https://doi.org/10.1177/09673911231171479).
- WELCH, B. L. The generalization of "Student's" problem when several different population variances are involved. *Biometrika*, v. 34, n. 1–2, p. 28–35, 1947. DOI: [10.1093/biomet/34.1-2.28](https://doi.org/10.1093/biomet/34.1-2.28).
