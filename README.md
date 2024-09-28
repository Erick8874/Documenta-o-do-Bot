## Documentação do Chatbot

Esta seção fornece informações sobre o funcionamento e a configuração do chatbot.

### Funcionalidades

- **Verificar todas as opções disponíveis**: O usuário consegue verificar todas as opções de moedas/criptomoedas disponíveis para consultas.
  
- **Consultar cotação de moedas**: O chatbot permite que o usuário consulte a cotação atual de diversas moedas, com a opção de escolher entre os valores de compra ou venda, convertendo para a moeda de preferência.

- **Consultar histórico de moedas**: O usuário pode verificar o histórico de variação de uma moeda ao longo de um período especificado ( últimos 3, 5 ou 7 dias).

- **Conversão de criptomoedas**: O chatbot também permite consultar o valor atual de criptomoedas e a sua variação percentual nas últimas 24 horas.

- **Integração com API de Moedas**: O chatbot utiliza uma API de consulta de moedas e criptomoedas em seu back-end para trazer informações em tempo real e de forma dinâmica. [API De Cotações Awesome](https://docs.awesomeapi.com.br/api-de-moedas)

- **Respostas em áudio**: O chatbot está integrado com a **API Serverless Framework** desenvolvida para retornar respostas em áudio, proporcionando uma experiência mais imersiva para os usuários que preferem interações por voz.

- **Integração com Slack**: O chatbot também possui integração com o Slack, permitindo que os usuários interajam com as funcionalidades diretamente na plataforma.


### Tabela de Interações do Chatbot

| Intent                  | Utterances                                                                                                  | Slots                                   | O que faz cada Intent                                                                  |
|-------------------------|-------------------------------------------------------------------------------------------------------------|-----------------------------------------|----------------------------------------------------------------------------------------|
| GetCurrencyRate         | "Quero verificar o valor de uma moeda","Quero ver o valor de uma moeda", "Me fale o valor de uma moeda", "Quanto está o {CurrencyType} hoje?" | `CurrencyType` `RateType` `DestinationCurrency` | Responde com a cotação atual da moeda desejada convertida para a moeda de preferência do usuário. |
| CurrencyHistoryIntent    | "Me fale o histórico da moeda nos últimos dias.", "Quero saber o histórico de uma moeda.", "Quero saber o histórico da {CurrencyType} nos últimos dias." | `CurrencyType` `RateType` `TimeRange`       | Retorna o valor da moeda escolhida a cada dia ao longo do período especificado.       |
| CryptoConversion        | "Me fale o valor de uma criptomoeda", "Quero saber o valor de uma criptomoeda", "Me fale o valor da {CryptoCurrency}" | `CryptoCurrency` `TargetCurrency` `DailyChange` | Retorna a cotação atual da criptomoeda desejada na moeda especificada e a variação de preço nas últimas 24 horas.                |
| CheckCurrenciesIntent               | "Consultar as moedas disponíveis", "Quais as opções de moedas ?"  , "Quais as moedas?", "Pesquisar as moedas"                                               | `Type` `NameCod` `DocumentationLink`               | Retorna todas as opções de criptomoedas e moedas disponíveis para consulta e a documentação do chatbot.           |


### Tipos de Slots e Valores Possíveis


| Tipo de Slot          | Valores Possíveis                           | Descrição                                                        | Exemplo    |
|-----------------------|---------------------------------------------|------------------------------------------------------------------|------------|
| `RateType`            | "Compra", "Venda"                           | Qual a cotação desejada (compra ou venda).                       | "Compra"   |
| `TimeRange`           | "3", "5", "7"                               | Número de dias do histórico.                                     | 5          |
| `CryptoType`          | "BTC", "ETH", "LTC", "XRP", "DOGE"        | Criptomoedas disponíveis para consulta.                          | "BTC"      |
| `CurrencyToCrypto`    | "BRL", "USD", "EUR"                         | Moedas disponíveis para verificar o valor de uma criptomoeda.    | "USD"      |
| `CurrencyType`        | "USD", "EUR", "GBP", "JPY", "BRL"         | Moedas disponíveis para consulta.                                | "USD"      |
| `DestinationCurrency` | "USD", "EUR", "GBP", "JPY", "BRL"         | Moeda que será feita a conversão do valor.                       | "BRL"      |
| `DailyChange`         | "Sim", "Não"                                | Se o usuário deseja ver a variação percentual de 24h da criptomoeda. | "Sim"      |
| `Type`                | "Moedas Normais", "Criptomoedas"          | Qual tipo de ativo (moedas ou criptomoedas) o usuário deseja visualizar as opções disponíveis. | "Criptomoedas" |
| `DocumentationLink`   | "Sim", "Não"                                | Se o usuário quer receber o link da documentação do chatbot ou não. | "Não"      |
| `NameCod`     | "Nome", "Código"                     | Qual a opção desejada para ver as moedas (nome ou sigla).         | "500"      |


## Banco de Dados


Para este projeto, foi utilizado o **Amazon DynamoDB** para armazenar informações sobre os arquivos de áudio gerados e armazenados no **S3**. Os seguintes campos são armazenados no DynamoDB:

- **unique_id**: ID único do registro gerado a partir da frase inserida na API.
- **created_audio**: Data e hora do registro no DynamoDB.
- **received_phrase**: Frase recebida pelo usuário no endpoint.
- **url_to_audio**: URL do arquivo de áudio armazenado no S3.

## Variáveis de Ambiente

Este projeto utiliza variáveis de ambiente para configuração. Para configurar seu ambiente local:

1. Crie no diretório `api-tts` um arquivo com o nome ".env"

2. Abra o arquivo .env e substitua as variáveis de exemplo pelos valores reais, seguindo as instruções fornecidas no próprio arquivo.

| Variável        | Descrição                                                            | Exemplo              |
|-----------------|----------------------------------------------------------------------|----------------------|
| `BUCKET_NAME`   | Nome do bucket utilizado para armazenamento dos áudios no S3.                   | `meu-bucket-s3`      |
| `BUCKET_FOLDER` | Pasta de armazenamento dentro do bucket S3.                          | `pasta-arquivos`     |
| `TABLE_NAME`    | Nome da tabela no DynamoDB utilizada para armazenar os registros.    | `tabela-registros`   |
