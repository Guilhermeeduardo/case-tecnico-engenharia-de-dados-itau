# Case Técnico — Engenharia de Dados | Itaú

## Objetivo

Investigar por que a satisfação dos clientes de investimentos caiu no último trimestre, identificar os produtos mais afetados e buscar fatores operacionais associados à deterioração.

## Pergunta de negócio

> Por que a satisfação dos clientes de investimentos caiu no último trimestre e o que pode ser feito a respeito?

## Fontes de dados

Foram utilizadas quatro fontes:

- `formulario_digital.csv` — respostas de satisfação dos clientes;
- `atendimento_manual.xlsx` — registros de atendimento;
- `extrato_sistema.csv` — indicadores operacionais;
- `dicionario_dados_case.xlsx` — documentação dos campos.

## Tratamento e integração dos dados

As bases apresentavam diferenças de formato entre identificadores de clientes, nomes de produtos e datas.

Os principais tratamentos realizados foram:

- padronização dos identificadores de clientes;
- normalização dos nomes dos produtos;
- conversão dos campos de data para `datetime`;
- tratamento de registros com datas inválidas;
- integração entre formulário e extrato.

Como um mesmo cliente pode aparecer diversas vezes nas bases, um `merge` apenas pelo identificador gerava multiplicação de registros.

Por isso, a associação foi realizada utilizando:

**cliente + produto + data mais próxima**, com tolerância máxima de **10 dias**.

Essa regra permitiu encontrar correspondência para aproximadamente **91% das pesquisas**.

## Principais achados

A queda de satisfação no 2026T2 não ocorreu de forma uniforme entre os produtos.

### Variação da satisfação — 2026T1 vs. 2026T2

| Produto | Variação |
|---|---:|
| Fundos | **-0,52** |
| Carteira Administrada | **-0,32** |
| CDB | -0,14 |
| Tesouro Direto | -0,03 |
| COE | +0,41 |

![Variação da satisfação por produto](images/variacao%20da%20satisfacao%20por%20produto.png)

Os maiores sinais de deterioração foram encontrados em **Fundos** e **Carteira Administrada**.

### Carteira Administrada

Entre 2026T1 e 2026T2:

- satisfação: **8,37 → 8,05**;
- reclamações médias em 90 dias: **0,30 → 0,63 (+111%)**;
- tempo médio de resolução: **41,1h → 82,3h (+100%)**;
- suitability pendente: **5,4% → 13,2% (+145%)**.
![Indicadores operacionais da Carteira Administrada](images/carteira%20administrativa.png)
Os dados indicam uma associação entre a redução da satisfação e uma forte deterioração dos indicadores operacionais.

### Fundos

Entre 2026T1 e 2026T2:

- satisfação: **8,70 → 8,18**;
- reclamações médias: **0,00 → 0,10**;
- tempo médio de resolução: **17,2h → 21,9h**;
- suitability pendente: **9,6% → 11,5%**.

Fundos apresentou a maior queda de satisfação, embora a deterioração operacional tenha sido mais moderada do que em Carteira Administrada.

## Recomendações

1. **Priorizar Carteira Administrada**, investigando os gargalos responsáveis pelo aumento no tempo de resolução e nas reclamações.
2. **Acompanhar Fundos**, devido à maior queda de satisfação observada no período.
3. **Criar monitoramento recorrente por produto**, acompanhando satisfação e indicadores operacionais em conjunto.
4. **Atuar preventivamente nas pendências de suitability**, principalmente em Carteira Administrada.

## Próximos passos

Como evolução da solução:

- aplicar técnicas de NLP aos comentários para identificar automaticamente os principais motivos de insatisfação;
- criar um pipeline de monitoramento contínuo dos indicadores;
- estabelecer alertas para deteriorações relevantes;
- avaliar custos de infraestrutura, processamento e manutenção;
- mensurar o impacto das ações implementadas e, quando possível, estimar retorno sobre o investimento.

## Limitações

- O relacionamento entre formulário e extrato foi construído por uma regra de associação e não por uma chave transacional explícita.
- Aproximadamente 91% das pesquisas encontraram correspondência válida.
- Oito registros do extrato possuíam data inválida e ficaram fora da integração temporal.
- A base de atendimento foi padronizada, mas não incorporada à análise principal por não haver uma regra temporal de associação suficientemente robusta nesta versão.
- Os resultados indicam **associação**, e não causalidade comprovada.

## Tecnologias utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Google Colab

## Uso de IA

Ferramentas de IA foram utilizadas como apoio para organização do raciocínio, revisão de código e estruturação da análise. As decisões de tratamento, validação, interpretação e conclusões foram verificadas com base nos dados do case.

## Arquivos

- `Case_Itau.ipynb` — notebook principal da análise;
- arquivos de dados fornecidos no case;
- apresentação executiva com os principais achados e recomendações.
