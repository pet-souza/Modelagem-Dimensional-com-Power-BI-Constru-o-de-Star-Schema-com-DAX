⭐ Sobre o Projeto
Este projeto foi desenvolvido como parte do Desafio de Modelagem Dimensional com Power BI da DIO.
O objetivo é transformar a tabela única Financials1 em um modelo estrela (Star Schema), criando tabelas dimensão e fato utilizando DAX, boas práticas de modelagem e organização de dados.

🎯 Objetivo
Criar tabelas dimensão e fato a partir da tabela original Financials1

Construir um modelo estrela funcional

Criar colunas calculadas e tabelas com DAX

Organizar o modelo para análises de vendas

Documentar o processo e publicar no GitHub

🗂️ Estrutura do Modelo Estrela
O modelo final contém:

📌 Tabela Fato
F_Vendas

📌 Tabelas Dimensão
D_Produtos

D_Produtos_Detalhes

D_Descontos

D_Detalhes

D_Calendario

📌 Tabela de Backup
Financials_origem (opcional)

🛠️ Etapas do Projeto
1️⃣ Importação da tabela Financials1
A tabela Financials1 contém os dados originais de vendas, produtos, preços, descontos, datas e informações de contexto.

2️⃣ Criação da tabela de backup (opcional)
DAX
Financials_origem =
Financials1
A tabela foi ocultada para evitar uso acidental.

3️⃣ Criação das Tabelas Dimensão
📌 D_Produtos
Criada por agrupamento (SUMMARIZE) e enriquecida com métricas (ADDCOLUMNS):

DAX
D_Produtos =
ADDCOLUMNS(
    SUMMARIZE(
        Financials1,
        Financials1[Product]
    ),
    "ID_Produto", RANKX(ALL(Financials1[Product]), MAX(Financials1[Product])),
    "Media_Unidades", AVERAGE(Financials1[Units Sold]),
    "Media_Valor_Venda", AVERAGE(Financials1[Sale Price]),
    "Mediana_Valor_Venda", VALUE(MEDIAN(Financials1[Sale Price])),
    "Max_Valor_Venda", MAX(Financials1[Sale Price]),
    "Min_Valor_Venda", MIN(Financials1[Sale Price])
)
📌 D_Produtos_Detalhes
DAX
D_Produtos_Detalhes =
SELECTCOLUMNS(
    Financials1,
    "ID_Produto", Financials1[Product],
    "Discount Band", Financials1[Discount Band],
    "Sale Price", Financials1[Sale Price],
    "Units Sold", Financials1[Units Sold],
    "Manufacturing Price", Financials1[Manufacturing Price]
)
📌 D_Descontos
DAX
D_Descontos =
SELECTCOLUMNS(
    Financials1,
    "ID_Produto", Financials1[Product],
    "Discount", Financials1[Discount],
    "Discount Band", Financials1[Discount Band]
)
📌 D_Detalhes
Contém informações complementares não contempladas nas outras dimensões:

DAX
D_Detalhes =
SELECTCOLUMNS(
    Financials1,
    "ID_Produto", Financials1[Product],
    "Segment", Financials1[Segment],
    "Country", Financials1[Country],
    "Salesperson", Financials1[Salesperson],
    "Profit", Financials1[Profit]
)
📌 D_Calendario
Criada com a função CALENDAR:

DAX
D_Calendario =
CALENDAR(
    MIN(Financials1[Date]),
    MAX(Financials1[Date])
)
Colunas adicionais:

DAX
Ano = YEAR(D_Calendario[Date])
Mes = MONTH(D_Calendario[Date])
NomeMes = FORMAT(D_Calendario[Date], "MMMM")
4️⃣ Criação da Tabela Fato
DAX
F_Vendas =
SELECTCOLUMNS(
    Financials1,
    "SK_ID", Financials1[Index],
    "ID_Produto", Financials1[Product],
    "Produto", Financials1[Product],
    "Units Sold", Financials1[Units Sold],
    "Sale Price", Financials1[Sale Price],
    "Discount Band", Financials1[Discount Band],
    "Segment", Financials1[Segment],
    "Country", Financials1[Country],
    "Salesperson", Financials1[Salesperson],
    "Profit", Financials1[Profit],
    "Date", Financials1[Date]
)
Se não existir a coluna Index:

DAX
Index =
RANKX(
    ALL(Financials1),
    Financials1[Product],
    ,
    ASC
)
🔗 5️⃣ Relacionamentos do Modelo Estrela
D_Produtos[Product] → F_Vendas[ID_Produto]

D_Produtos_Detalhes[ID_Produto] → F_Vendas[ID_Produto]

D_Descontos[ID_Produto] → F_Vendas[ID_Produto]

D_Detalhes[ID_Produto] → F_Vendas[ID_Produto]

D_Calendario[Date] → F_Vendas[Date]

Todos os relacionamentos são 1 → muitos, com as dimensões no lado 1.

🧩 Funções DAX Utilizadas
SUMMARIZE

ADDCOLUMNS

SELECTCOLUMNS

CALENDAR

YEAR / MONTH / FORMAT

RANKX

MEDIAN / VALUE

MAX / MIN / AVERAGE

🖼️ Imagem do Modelo Estrela
Inclua aqui o print da aba Model do Power BI.

📦 Arquivos no Repositório
modelo.pbix

README.md

imagem_modelo_estrela.png

🚀 Conclusão
Este projeto demonstra a construção de um modelo dimensional completo utilizando DAX no Power BI, seguindo boas práticas de modelagem e organização de dados.
O modelo estrela facilita análises de vendas, produtos, descontos e desempenho comercial.
