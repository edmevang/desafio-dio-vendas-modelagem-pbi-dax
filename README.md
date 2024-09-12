# Desafio de Projeto: Modelo Star Schema a partir da Tabela Única Financial Sample

## Descrição do Desafio

Este projeto tem como objetivo a criação de um modelo de dados baseado em **Star Schema**, a partir de uma única tabela de amostra financeira chamada "Financial Sample". Utilizaremos a tabela original para gerar tabelas de dimensões e fatos, organizando os dados de forma a facilitar análises de vendas, produtos e descontos.

## Etapas do Projeto

### 1. **Cópia da Tabela Original**
   - A tabela original foi copiada para uma nova tabela chamada **Financials_origem**, que será mantida em modo oculto. Essa tabela serve como backup para o projeto e não será visível no modelo final.

### 2. **Criação das Tabelas de Dimensões**
   A partir da tabela original, criamos as seguintes tabelas de dimensões:

   - **D_Produtos**
     - Contém dados sobre produtos, com as seguintes colunas:
       - `ID_produto`
       - `Produto`
       - `Média de Unidades Vendidas`
       - `Média do valor de vendas`
       - `Mediana do valor de vendas`
       - `Valor máximo de Venda`
       - `Valor mínimo de Venda`

   - **D_Produtos_Detalhes**
     - Detalha informações adicionais sobre os produtos, como:
       - `ID_produto`
       - `Discount Band`
       - `Sale Price`
       - `Units Sold`
       - `Manufacturing Price`

   - **D_Descontos**
     - Centraliza informações sobre os descontos aplicados:
       - `ID_produto`
       - `Discount`
       - `Discount Band`

   - **D_Detalhes**
     - Fornece informações adicionais que não foram contempladas nas outras tabelas de dimensão, detalhando aspectos importantes das vendas, tais como:
       - `Segment`
       - `Country`
       - `Sales`
       - `Profit`

   - **D_Calendário**
     - Criada utilizando a função DAX `CALENDARAUTO()`, contém apenas o campo:
       - `Date`

### 3. **Criação da Tabela Fato**
   A tabela **F_Vendas** foi criada para armazenar os dados das vendas, sendo composta por colunas essenciais para análise, como:
   - `SK_ID`
   - `ID_Produto`
   - `Produto`
   - `Units Sold`
   - `Sales Price`
   - `Discount Band`
   - `Segment`
   - `Country`
   - `Sales`
   - `Profit`
   - `Date`

### 4. **Relacionamentos**
   Utilizando um esquema estrela, os relacionamentos entre a tabela fato (**F_Vendas**) e as tabelas de dimensões foram estabelecidos. O diagrama de relacionamento entre as tabelas foi gerado conforme o modelo descrito abaixo:

   ```mermaid
   erDiagram
       F_Vendas ||--o{ D_Calendario : "Date"
       F_Vendas ||--o{ D_CALENDARIO : "Date"
       F_Vendas ||--o{ D_Descontos : "ID_Produto"
       F_Vendas ||--o{ D_Detalhes : "SK_ID"
       F_Vendas ||--o{ D_Produtos : "ID_Produto"
       F_Vendas ||--o{ D_Produtos_Detalhes : "ID_Produto"
```

### 5. **DER**

   ```mermaid
erDiagram
    F_Vendas {
        int SK_ID
        string Product
        int ID_Produto
        int Units_Sold
    }
    D_Calendario {
        date Date
        decimal Sale_Price
        string Discount_Band
        string Segment
        string Country
        decimal Sales
        decimal Profit
        date Date
        string Month_Name
        int Year
    }
    D_CALENDARIO {
        date Date
    }
    D_Descontos {
        int ID_produto
        string Discount_Band
        decimal Discounts
    }
    D_Detalhes {
        string Segment
        string Country
        string Product
        string Discount_Band
        int Units_Sold
        decimal Manufacturing_Price
        decimal Sale_Price
        decimal Sales
        decimal COGS
        decimal Profit
        date Date
        int Month_Number
        string Month_Name
        int Year
        decimal Gross_Sales
        decimal Discounts
        int ID_produto
    }
    D_Produtos {
        string Product
        decimal Valor_minimo_de_venda
        decimal Valor_maximo_de_venda
        decimal Mediana_do_valor_de_vendas
        decimal Media_do_valor_de_vendas
        int ID_produto
        decimal Media_de_unidades_Vendidas
    }
    D_Produtos_Detalhes {
        string Discount_Band
        int Units_Sold
        decimal Manufacturing_Price
        decimal Sale_Price
        int ID_produto
    }

    %% Definindo os relacionamentos entre as tabelas
    F_Vendas ||--o{ D_Calendario : "Date"
    F_Vendas ||--o{ D_CALENDARIO : "Date"
    F_Vendas ||--o{ D_Descontos : "ID_Produto"
    F_Vendas ||--o{ D_Detalhes : "SK_ID"
    F_Vendas ||--o{ D_Produtos : "ID_Produto"
    F_Vendas ||--o{ D_Produtos_Detalhes : "ID_Produto"
```