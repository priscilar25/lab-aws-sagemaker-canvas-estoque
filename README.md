# 📊 Previsão de Estoque Inteligente na AWS com [SageMaker Canvas](https://aws.amazon.com/pt/sagemaker/canvas/)

Este projeto, intitulado 'Previsão de Estoque Inteligente na AWS com SageMaker Canvas', é um laboratório (Lab) desenvolvido pela DIO em parceria com a Nexa. Seu objetivo principal é utilizar o SageMaker Canvas para gerar previsões de estoque com base em técnicas de Machine Learning (ML).

## 📋 Pré-requisitos

Antes de começar, é necessário criar uma conta na AWS. 

A plataforma oferece  gratuitamente  2 meses para utilizar o SageMaker.

Para isso, é necessário atender as seguintes as configurações:

•250 horas por mês de ml.t3.medium em cadernos Studio OU 250 horas por mês de ml.t2.medium ou ml.t3.medium em instâncias de cadernos sob demanda

•25 horas por mês em ml.m5.4xlarge no SageMaker Data Wrangler

•10 milhões de unidades de gravação, 10 milhões, unidades de leitura, 25 GB de armazenamento por mês na SageMaker Feature Store

•50 horas por mês de instâncias m4.xlarge ou m5.xlarge em Treinamento

•125 horas de instância m4.xlarge ou m5.xlarge por mês no Inference

Caso contrário, serão cobradas taxas adicionais. 

-Sugiro seguir os seguintes links, fornecidos pela própria AWS:

https://aws.amazon.com/pt/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all&all-free-tier.q=sagemaker&all-free-tier.q_operator=AND

https://aws.amazon.com/pt/getting-started/hands-on/machine-learning-tutorial-generate-predictions-without-writing-code/

https://aws.amazon.com/pt/sagemaker/?did=ft_card&trk=ft_card

## ATENÇÃO: AO SAIR DA PLATAFORMA, DESCONECTE TODOS OS SERVIÇOS PARA NÃO SEREM COBRADOS !! ##

Não esqueça de clicar em *Log out* ao sair da guia do SageMaker Canvas (canto inferior esquerdo). Esse é o primeiro passo para encerrar os serviços.

## 🎯 Objetivos Deste Desafio de Projeto (Lab)

![image](https://github.com/digitalinnovationone/lab-aws-sagemaker-canvas-estoque/assets/730492/72f5c21f-5562-491e-aa42-2885a3184650)

-O SageMaker Canvas é uma ferramenta poderosa que permite realizar todo o processamento de dados sem a necessidade de codificação. Embora essa ferramenta automatize grande parte do trabalho, recomendo fortemente um aprofundamento nos conceitos de programação e no entendimento das métricas geradas. Essa base sólida permitirá uma compreensão mais profunda do processo completo de Machine Learning, que envolve desde o pré-processamento dos dados e a construção do modelo até a análise dos resultados e a geração de previsões.


## 🚀 Passo a Passo

### 1. Selecionar Dataset


O Dataset selecionado possui as seguintes informações:

-ID_PRODUTO: contém valores de únicos que identificam os produtos;

-DATA_EVENTO: contém a data da venda,em formato datetime;

-PRECO: contém o preco do produto;

-FLAG_PROMOCAO: contém valores 0 e 1, representando produto em promoção;

-QUANTIDADE_ESTOQUE: contém a quantidade de determinado produto em estoque

Antes de iniciar a construção do modelo, os dados foram verificados e limpos e removidos valores duplicados.


### 2. Construir/Treinar

- Para a Construção do modelo, a coluna alvo de predição "QUANTIDADE_ESTOQUE" foi selecionada, o próprio SageMaker Canvas sugere o modelo mais adequado, uma Série Temporal (indicado para previsão de valores futuros), onde é necessário identificar a coluna com valores que representam os produtos e tem a opção de incluir particularidades de calendário, como os feriados.
- Para a construção do modelo foi utilizado o Standard Build para obter maior precisão.


### 3. Analisar

-Após o treinamento do modelo anteriormente selecionado, obteve-se as seguintes métricas:

![metricas_SageMakerCanvas](https://github.com/user-attachments/assets/9e87694d-3883-4710-a356-d9436c73d57d)

•AVG wQL: 0.138 ( Avalia a previsão calculando a média da precisão nos quantis P10, P50 e P90. Um valor mais baixo indica um modelo mais preciso.)

•MAPE: 0.212 (MAPE é a média das diferenças absolutas entre os valores reais e os valores previstos ou estimados, dividida pelos valores reais e expressos em porcentagem. Um MAPE menor indica melhor desempenho, pois significa que os valores previstos ou estimados estão mais próximos dos valores reais.)

•WAPE: 0.187 (A soma do erro absoluto normalizado pela soma da meta absoluta, que mede o desvio geral dos valores previstos dos valores observados. Um valor menor indica um modelo mais preciso, onde WAPE = 0 é um modelo sem erros.)

•RMSE: 20.364 (Mede a raiz quadrada da diferença quadrada entre os valores previstos e reais e é calculada a média de todos os valores. Ela é usada para entender o erro de predição do modelo e é uma métrica importante para indicar a presença de grandes erros e discrepâncias no modelo. Os valores variam de zero (0) ao infinito, com números menores indicando um melhor ajuste do modelo aos dados. O RMSE depende da escala e não deve ser usado para comparar conjuntos de dados de diferentes tipos.)

•MASE: 0.00 (O erro médio absoluto da previsão normalizado pelo erro médio absoluto de um método simples de previsão de linha de base. Um valor mais baixo indica um modelo mais preciso, onde MASE < 1 é estimado como melhor do que a linha de base e MASE > 1 é estimado como pior do que a linha de base.)

- A seguir foi avaliado o a influência dos fatores PROMOÇÃO e FERIADOS no Brasil dentro do modelo de predição de quantidade de estoque.

![influencia_SageMakerCanvas](https://github.com/user-attachments/assets/c4cb6a80-e0ed-4ae0-8f23-1fc03fc32d05)


•FLAG_PROMOCAO: 9.98%

•FERIADO_BR: 72.64%

Em suma, os resultados obtidos indicam que o modelo, em sua maioria, apresentou boa precisão nas previsões, apesar de algumas discrepâncias. A análise dos resultados evidenciou a influência significativa dos feriados nas previsões do modelo, enquanto o impacto das promoções mostrou-se menos expressivo.


### 4. Prever

Após o ajuste do modelo, conseguimos gerar previsões de estoque para cada produto, considerando três cenários distintos: otimista, pessimista e neutro. Essa abordagem permite uma análise mais robusta da demanda, auxiliando na tomada de decisões estratégicas relacionadas à gestão de estoque, como a definição de níveis de estoque de segurança e a consideração de fatores sazonais.

-A seguir, apresentamos um caso prático de previsão de demanda para o produto com código 1016, como exemplo da aplicação do modelo em nosso conjunto de dados:

![ID_1016](https://github.com/user-attachments/assets/cb563674-615c-4372-973b-4d0786fa05c1)

![ID_1016-Valores](https://github.com/user-attachments/assets/b10ceebc-8e04-4bf1-b910-65b1f3dab4a4)

 Conclui-se que, com base no histórico de demanda de 73 produtos, o nível de estoque ideal para o produto 1016 varia de acordo com o cenário. Recomenda-se manter um estoque de aproximadamente 70 unidades para um cenário otimista, 65 unidades para um cenário neutro e 59 unidades para um cenário pessimista.

 
## Funcionalidades SageMaker Canvas

- Dentro do SageMaker é possível visualizar os dados em diferentes formas:

• Gráfico (barras)

![grafico_SageMakerCanvas](https://github.com/user-attachments/assets/e542f526-9671-4555-83c5-c642a8fb3d79)

- É possível ter uma visão gráfica geral do Dataset, criar filtros e realizar operações para gerar uma nova coluna baseado no que se deseja analisar:

• Filtros

![filtro_SageMakerCanvas](https://github.com/user-attachments/assets/920f9a54-7a3e-4524-a2a6-d6f2306d6b25)

- É possível analisar a correlação entre as variáveis do Dataset:
  
  Para comparações de Spearman e Pearson, você pode definir o intervalo de correlações de filtro em qualquer ponto entre de -1 a 1, com 0 significando que não há 
  correlação. -1 e 1 significam que as variáveis têm uma forte correlação negativa ou positiva, respectivamente.
  

• Correlação 

![correlacao_SageMakerCanvas](https://github.com/user-attachments/assets/9106b3f7-f619-4a6b-b01e-253c8c48d33a)

-A baixa correlação entre promoções e estoque sugere que as estratégias promocionais atuais podem não estar sendo eficazes em impulsionar as vendas. As equipes de marketing devem reavaliar seus planos, testando diferentes formatos de promoções, públicos-alvo e canais de comunicação. A análise de dados pode auxiliar na identificação de oportunidades para criar campanhas mais personalizadas e relevantes para os consumidores.


## Links úteis 

https://docs.aws.amazon.com/ec2/latest/instancetypes/ec2-instance-regions.html#instance-types-us-east-1

https://docs.aws.amazon.com/vpc/latest/userguide/delete-vpc.html#delete-vpc-console

https://aws.amazon.com/pt/tutorials/machine-learning-tutorial-set-up-sagemaker-studio-account-permissions/

https://docs.aws.amazon.com/pt_br/sagemaker/latest/dg/canvas-explore-data-analytics.html
