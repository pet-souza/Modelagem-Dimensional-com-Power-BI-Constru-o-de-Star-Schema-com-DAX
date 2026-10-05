# Modelagem-Dimensional-com-Power-BI-Constru-o-de-Star-Schema-com-DAX
Projeto de modelagem dimensional no Power BI, transformando a tabela Financials1 em um modelo estrela com tabelas fato e dimensão criadas via DAX. Inclui documentação, diagrama do modelo e boas práticas para análise de vendas.

⭐ Sobre o Projeto Este projeto foi desenvolvido como parte do Desafio de Modelagem Dimensional com Power BI da DIO. O objetivo é transformar a tabela única Financials1 em um modelo estrela (Star Schema), criando tabelas dimensão e fato utilizando DAX, boas práticas de modelagem e organização de dados.

🎯 Objetivo Criar tabelas dimensão e fato a partir da tabela original Financials1

Construir um modelo estrela funcional

Criar colunas calculadas e tabelas com DAX

Organizar o modelo para análises de vendas

Documentar o processo e publicar no GitHub

🗂️ Estrutura do Modelo Estrela O modelo final contém:

📌 Tabela Fato F_Vendas

📌 Tabelas Dimensão D_Produtos

D_Produtos_Detalhes

D_Descontos

D_Detalhes

D_Calendário

📌 Tabela de Backup Financeiro_origem (opcional)

🛠️ Etapas do Projeto 

1️⃣ Importação da tabela Financials1 A tabela Financials1 contém os dados originais de vendas, produtos, preços, descontos, dados e informações de contexto.

2️⃣ Criação da tabela de backup (opcional) DAX Financials_origem = Financials1 Uma tabela foi ocultada para evitar uso acidental.

3️⃣ Criação das Tabelas Dimensão 
📌 D_Produtos Criados por agrupamento (SUMMARIZE) e enriquecida com métricas (ADDCOLUMNS):
DAX D_Produtos = ADDCOLUMNS( SUMMARIZE( Finanças1, Finanças1[Produto] ), "ID_Produto", RANKX(ALL(Finanças1[Produto]), MAX(Finanças1[Produto])), "Media_Unidades", AVERAGE(Finanças1[Unidades Vendidas]), "Media_Valor_Venda", AVERAGE(Finanças1[Venda Preço]), "Mediana_Valor_Venda", VALUE(MEDIAN(Finanças1[Preço Venda])), "Max_Valor_Venda", MAX(Finanças1[Preço Venda]), "Min_Valor_Venda", MIN(Finanças1[Preço Venda]) ) 
📌 D_Produtos_Detalhes DAX D_Produtos_Detalhes = SELECTCOLUMNS( Finanças1, "ID_Produto", Finanças1[Produto], "Desconto Banda", Finanças1[Banda de Desconto], "Preço de Venda", Finanças1[Preço de Venda], "Unidades Vendidas", Finanças1[Unidades Vendidas], "Preço de Fabricação", Finanças1[Preço de Fabricação] ) 
📌 D_Descontos DAX D_Descontos = SELECTCOLUMNS( Finanças1, "ID_Produto", Finanças1[Produto], "Desconto", Finanças1[Desconto], "Desconto Band", Financials1[Discount Band] ) 
📌 D_Detalhes Contém informações complementares não contempladas nas outras dimensões:
DAX D_Detalhes = SELECTCOLUMNS( Financials1, "ID_Produto", Financials1[Product], "Segment", Financials1[Segment], "Country", Financials1[Country], "Vendedor", Financials1[Vendedor], "Profit", Financials1[Profit] ) 
📌 D_Calendario Criado com a função CALENDAR:
DAX D_Calendario = CALENDAR( MIN(Financials1[Date]), MAX(Financials1[Date]) ) Colunas adicionais:
DAX Ano = YEAR(D_Calendario[Date]) Mes = MONTH(D_Calendario[Date]) NomeMes = FORMAT(D_Calendario[Date], "MMMM") 

4️⃣ Criação da Tabela Fato DAX F_Vendas = SELECTCOLUMNS( Financials1, "SK_ID", Financials1[Index], "ID_Produto", Financials1[Product], "Produto", Financeiro1[Produto], "Unidades Vendidas", Financeiro1[Unidades Vendidas], "Preço de Venda", Financeiro1[Preço de Venda], "Faixa de Desconto", Financeiro1[Faixa de Desconto], "Segmento", Financeiro1[Segmento], "País", Financeiro1[País], "Vendedor", Financeiro1[Vendedor], "Lucro", Financeiro1[Lucro], "Data", Financeiro1[Data] ) Se não existir a coluna Índice: Índice DAX = RANKX( ALL(Finanças1), Finanças1[Produto], , ASC ) 🔗 

5️⃣ Relacionamentos do Modelo Estrela D_Produtos[Produto] → F_Vendas[ID_Produto]

D_Produtos_Detalhes[ID_Produto] → F_Vendas[ID_Produto]

D_Descontos[ID_Produto] → F_Vendas[ID_Produto]

D_Detalhes[ID_Produto] → F_Vendas[ID_Produto]

D_Calendário[Data] → F_Vendas[Data]

Todos os relacionamentos são 1 → muitos, com as dimensões no lado 1.

🧩 Funções DAX Utilizadas SUMMARIZE

ADICIONARCOLUNAS

SELECIONAR COLUNAS

CALENDÁRIO

ANO / MÊS / FORMATO

RANKX

MEDIANA / VALOR

MÁXIMO / MÍNIMO / MÉDIA

🖼️ Imagem do Modelo Estrela Inclua aqui o print da aba Modelo do Power BI.

📦 Arquivos no Repositório modelo.pbix

README.md

imagem_modelo_estrela.png

🚀 Conclusão Este projeto demonstra a construção de um modelo dimensional completo utilizando DAX no Power BI, seguindo boas práticas de modelagem e organização de dados. O modelo estrela facilita análises de vendas, produtos, descontos e desempenho comercial.
