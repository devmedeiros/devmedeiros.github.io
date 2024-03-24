---
title: Limpando Base de Dados para Classificação de Score de Crédito
date: 2022-07-24 13:06:00 -0300
categories: [Blog]
keywords: [limpeza de dados, python, pandas, tutorial, kaggle, numpy, score, classificação]
tags: [Limpeza de Dados, Python, Tutorial]
showtoc: true
summary: Como surgir com formas de limpar um banco de dados usando Python
cover:
    image: "https://ik.imagekit.io/devmedeiros/cleaning_6qcrJxQyu.webp?tr=w-700"
    alt: "um homem usando luvas limpando uma mesa com um pano"
    caption: "Profession photo criada por freepik - freepik.com"
---

> **Aviso:** Eu estarei falando sobre como chegar no código python, caso queira ler o código em si, dirija-se a este [repositório](https://github.com/devmedeiros/credit-score-classification-app/tree/main/notebooks).

## Conheça a Base para Classificação de Score de Crédito

A base de dados que vamos limpar vem do [kaggle](https://www.kaggle.com/datasets/parisrohan/credit-score-classification?select=train.csv), a qual está no arquivo `train.csv`, mas os passos que iremos falar também podem ser usados para `test.csv`.

Há 28 colunas e 100 mil linhas neste banco de dados. Eu compilei uma descrição das variáveis na tabela a seguir.

| Variável | Descrição |
|---|----|
|`ID` | Representa uma identificação única de uma entrada |
|`Customer_ID` | Representa uma identificação única de uma pessoa |
|`Month` | Representa o mês do ano |
|`Name` | Representa o nome de uma pessoa |
|`Age` | Representa a idade de uma pessoa |
|`SSN` | Representa o número social de uma pessoa americana |
|`Occupation` | Representa a profissão de uma pessoa |
|`Annual_Income` | Representa o salário anual de uma pessoa |
|`Monthly_Inhand_Salary` | Representa o salário mensal base de uma pessoa |
|`Num_Bank_Accounts` | Representa quantas contas bancárias uma pessoa possui |
|`Num_Credit_Card` | Representa o número de cartões de crédito que uma pessoa possui |
|`Interest_Rate` | Representa a taxa de juros de um cartão de crédito |
|`Num_of_Loan` | Representa quantos empréstimos uma pessoa fez com o banco |
|`Type_of_Loan` | Representa os tipos de empréstimos feitos por uma pessoa |
|`Delay_from_due_date` | Representa o numero médio de dias atrasados da data de pagamento |
|`Num_of_Delayed_Payment` | Representa o numério médio de pagamentos atrasados feitos por uma pessoa |
|`Changed_Credit_Limit` | Representa, em porcentagem, a mudança de limite do cartão de crédito |
|`Num_Credit_Inquiries` | Representa o número de consultas de cartão de crédito |
|`Credit_Mix` | Representa a classificação da mistura de créditos |
|`Outstanding_Debt` | Representa a quantidade de saldo devedor a ser pago em dólares americanos |
|`Credit_Utilization_Ratio` | Representa a proporção de utilização de cartões de crédito |
|`Credit_History_Age` | Representa a idade do histórico de uso de crédito de uma pessoa |
|`Payment_of_Min_Amount` | Representa se uma pessoa pagou apenas o mínimo |
|`Total_EMI_per_month` | Representa os pagamentos das parcelas mensais de empréstimos em dólares americanos |
|`Amount_invested_monthly` | Representa a quantidade de dinheiro investido pelo cliente em dólares americanos |
|`Payment_Behaviour` | Representa o comportamento de pagamento do cliente em dólares americanos |
|`Monthly_Balance` | Representa o extrato mensal do cliente em dólares americanos |
|`Credit_Score` | Representa as faixas de score de crédito (Baixo, Médio, Bom)

Mesmo tendo 100 mil linhas, dentro dessas linhas existem apenas 12.500 clientes diferentes, cada cliente aparece 8 vezes (de Janeiro a Agosto). Então basicamente nós podemos selecionar um cliente específico e olhar as informações dele e facilmente identificar dados incorretos e somos capazes de ajustá-los.

## Lidando com Erros de Digitação e Outliers

Neste banco de dados há diversos erros de digitação ou simplesmente coisas que não fazem sentido. Você irá encontrar valores que são: `_`, `!@9#%8`, `__10000__`, `NM` ou `_______`. Eu acredito que esses erros estão na base para representar o caos que você pode encontrar quando estiver lidando com dados reais e a maioria deles significam que esse valor é nulo.

Por um momento eu pensei que `__10000__` poderia ser apenas um erro de digitação, mas não há nenhuma quantidade de dinheiro investida mensalmente que é superior a 200 dólares americanos.

```txt
__10000__             4305
0.0                    169
80.41529543900253        1
36.66235139442514        1
89.7384893604547         1
                      ... 
36.541908593249026       1
93.45116318631192        1
140.80972223052834       1
38.73937670100975        1
167.1638651610451        1
Name: Amount_invested_monthly, Length: 91049, dtype: int64
```

Seguindo essa lógica, eu procurei por mais coisas que não faziam sentido no data frame e comecei a substituir esses valores pelos `nan`s do numpy. Eu também procurei por outliers olhando a distribuição dos valores, se havia um valor que aparecia apenas uma vez e ele estava isolado, eu o substituia por um valor nulo. Eu não baseiei essa decisão apenas nisso, eu também procurei por clientes que tinham esse outlier e observei todos os dados desse cliente específico, e eu sempre encontrava coisas estranhas como:

![uma tabela mostrando informação sobre um cliente específico e mostrando um outlier na sua receita anual](https://raw.githubusercontent.com/devmedeiros/credit-score-classification-app/main/reports/figures/annual_income.png#center)

Olhando para este cliente em específico é fácil ver que ele não ganhou todo esse dinheiro anualmente apenas um mês do ano.

Ao terminar essa busca por erros de digitação e outliers, não esqueça de passar o novo tipo de dados para suas variáveis. Algumas variáveis como `age` começaram com caracteres string entre as idades e por isso ao ser feito a leitura dos dados o pandas os reconheceu como objeto e não como tipo int ou float.

## Preenchendo Valores em Branco

Depois de lidar com todos os outliers e erros de digitação, nós terminamos com diversos valores nulos, como pode ser visto a baixo:

```python
df.isna().sum()[df.isna().sum() > 0]
```
```txt
Age                         2776
Occupation                  7062
Annual_Income                993
Monthly_Inhand_Salary      15002
Num_Credit_Card             2271
Interest_Rate               2034
Num_of_Loan                 4348
Type_of_Loan               11408
Num_of_Delayed_Payment      7002
Changed_Credit_Limit        2091
Num_Credit_Inquiries        1965
Credit_Mix                 20195
Credit_History_Age          9030
Payment_of_Min_Amount      12007
Amount_invested_monthly     8784
Payment_Behaviour           7600
Monthly_Balance             1200
dtype: int64
```

Ao invés de apagar todos os valores em branco eu primeiro tento preencher eles usando informação que já temos disponível. Lembra que eu mencionei que um cliente possui dados históricos de 8 meses atrás? Nós podemos apenas usar esses dados históricos para preencher os valores nulos usando uma medida resumo da nossa escolha filtrando para o cliente, isso será mais preciso do que apenas calcular a média do nosso banco de dados.

Eu decidir usar o valor médio para as seguintes colunas:

```python
mean_columns = [
    'Num_of_Delayed_Payment', 'Changed_Credit_Limit', 'Num_Credit_Inquiries',
    'Amount_invested_monthly', 'Monthly_Balance', 'Num_of_Loan',
    'Num_Credit_Card', 'Interest_Rate', 'Annual_Income', 'Monthly_Inhand_Salary'
    ]
```

E o último valor não nulo para estas:

```python
last_columns = ['Age', 'Occupation', 'Type_of_Loan', 'Credit_Mix']
```

O motivo de não usar a média para todos os meus valores é porque eu não queria ter que lidar com uma pessoa tendo 20,5 anos de idade e `Occupation`, `Type_of_Loan`, e `Credit_Mix` são dados discretos.

## Engenharia das Features

Com os dados limpos, nós podemos seguir com a engenharia das features. Vamos começar com a variável `Type_of_Loan`, que possui algumas ocorrências em que em uma única célula possui diversos tipos de empréstimos, como você pode ver:

```txt
Not Specified, Mortgage Loan, Auto Loan, and Payday Loan                                                                                         8
Payday Loan, Mortgage Loan, Debt Consolidation Loan, and Student Loan                                                                            8
Debt Consolidation Loan, Auto Loan, Personal Loan, Debt Consolidation Loan, Student Loan, and Credit-Builder Loan                                8
Student Loan, Auto Loan, Student Loan, Credit-Builder Loan, Home Equity Loan, Debt Consolidation Loan, and Debt Consolidation Loan               8
Personal Loan, Auto Loan, Mortgage Loan, Student Loan, and Student Loan                                                                          8
Name: Type_of_Loan, Length: 5380, dtype: int64
```

Então eu vou salvar todos os tipos de empréstimos diferentes em um vetor, separando todos os empréstimos sempre que surgir uma `,` ou `, and`.

```python
loan_types = []
for index in df.index:
    temp = df.Type_of_Loan[index].replace('and ', '').split(', ')
    for i in temp: #loan in temp array
        if i not in loan_types: #if loan is not in loan_types
            loan_types.append(i) #add it
```

Agora podemos criar variáveis dummies usando `loan_types`, assim um cliente recebe o número 1 se ele tiver esse empréstimo ou 0 caso não tenha.

```python
for loan in loan_types:
    df[loan] = 0 #create the loan column in the df with 0
    for index in df.index:
        temp = df.Type_of_Loan[index].replace('and ', '').split(', ')
        if loan in temp:
            df.loc[index, loan] = 1
```

Agora eu continuo trabalhando nesse banco para o tornar pronto para o treinamento de um modelo de aprendizado de máquina. Para isso, eu preciso transformar todos os meus dados discretos em numéricos.

A variável `Credit_History_Age` tem os valores como strings "22 Years and 5 Months" e esse padrão se repete, então podemos aproveitar isso e selecionar o ano multiplicado por 12 e somar o mês, resultando em um novo recurso com o crédito idade da história em meses. Quando acabarmos com isso ainda haverá valores nulos e, para preenchê-los, escolho interpolar os valores. Isso funciona muito bem quando o valor ausente é de fevereiro até julho porque interpola com a idade do histórico de crédito do cliente, mas torna-se uma suposição incorreta quando o valor ausente é em janeiro ou agosto.

Os nomes dos meses serão substituidos pelo seu representante numérico, logo janeiro será 1, fevereiro será 2, e assim por diante. `credit_mix` e `credit_score` possuem 3 categorias sequenciais, eu escolhi usar -1, 0, e 1, mas você também pode usar 1, 2, 3 e irá produzir o mesmo resultado.

Não se esqueça de checar o [repositório](https://github.com/devmedeiros/credit-score-classification-app/tree/main/) no GitHub caso queira ver o código completo mencionado aqui e para baixar a base tratada.