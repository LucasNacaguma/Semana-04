# Semana 4 — Modelagem de Dados

Nesta semana, o objetivo é compreender como estruturar dados saindo das regras de negócio até o banco de dados físico, e diferenciar modelos otimizados para transações daqueles voltados para análise. Além da teoria, a prática consiste em desenhar modelos para um domínio simples de e-commerce utilizando código.

## Objetivos de aprendizagem

Ao final da semana, espera-se que você consiga:

- diferenciar sistemas OLTP de sistemas OLAP;
- identificar e aplicar os elementos principais de um diagrama ER (Entidade-Relacionamento);
- mapear um modelo relacional para um banco de dados físico;
- entender o propósito da modelagem dimensional para o mundo de *Analytics*;
- classificar e criar tabelas fato e tabelas dimensão;
- desenhar um Esquema Estrela (*Star Schema*);
- gerar representações visuais dos modelos utilizando a biblioteca `graphviz` em Python.

---

## 1. OLTP vs. OLAP

Antes de modelar, precisamos entender o propósito do banco de dados. A modelagem muda drasticamente dependendo de como os dados serão consumidos.

| Característica | OLTP (Sistemas Transacionais) | OLAP (Sistemas Analíticos) |
|---|---|---|
| **Foco** | Inserção, atualização e exclusão rápida. | Leitura rápida e agregação de grandes volumes. |
| **Modelagem** | Relacional (Diagrama ER). Altamente normalizado. | Dimensional (Esquema Estrela). Desnormalizado. |
| **Usuário final**| Aplicações, clientes do sistema. | Analistas de dados, Cientistas de dados, BI. |

## 2. Modelagem Entidade-Relacionamento (ER)

Focada em sistemas OLTP, tudo começa com o entendimento das regras de negócio. O modelo conceitual foca no "O QUÊ" (alto nível), enquanto o modelo lógico foca no "COMO" (detalhamento técnico independente do banco).

Os elementos fundamentais incluem:
- **Entidades:** Representam os objetos do mundo real (ex: Cliente, Produto).
- **Atributos:** As características das entidades (ex: Nome, Preço).
- **Relacionamentos:** Como as entidades interagem (ex: Cliente 'realiza' Pedido). Podem ter cardinalidades como 1:1, 1:N (Um para Muitos) ou N:M (Muitos para Muitos).

## 3. Mapeamento: Modelo para Banco de Dados

Ao converter o modelo lógico para um banco de dados físico, seguimos regras rigorosas de conversão:

| Elemento lógico | Elemento físico | Descrição |
|---|---|---|
| **Entidade** | Tabela | Cada entidade do modelo lógico é convertida em uma tabela. |
| **Atributo** | Coluna | Tornam-se as colunas da tabela, com tipos de dados definidos. |
| **Relacionamento**| Chave | Implementados usando Chaves Primárias (PK) e Estrangeiras (FK). |

## 4. O Mundo de Analytics e Esquema Estrela

Saindo das transações diárias, entramos no *Data Warehouse*. A **modelagem dimensional** permite "fatiar" (*slice and dice*) os dados por diferentes perspectivas de negócios.

- **Tabela Fato:** Armazena as métricas e eventos (ex: Quantidade Vendida, Receita Total). Contém as chaves estrangeiras.
- **Tabela Dimensão:** Fornece o contexto descritivo para os fatos (Quem, O que, Onde, Quando).
- **Granularidade:** É o nível de detalhe de uma única linha da Tabela Fato (ex: 1 linha = 1 item de um pedido).

O **Esquema Estrela (Star Schema)** é o design mais comum para isso: uma única Tabela Fato no centro, com múltiplas Tabelas Dimensão ligadas diretamente a ela nas pontas. Isso minimiza `joins` complexos, acelerando consultas de agregação.

## 5. Prática: Diagramas como Código (graphviz)

Na prática desta semana, desenhamos os modelos para o domínio de E-commerce. Para garantir um visual profissional, reprodutível e versionável, utilizamos a biblioteca Python `graphviz`.

O método utiliza "Diagramas como Código", injetando tabelas HTML dentro dos nós do grafo e usando o método `edge` para criar os relacionamentos (chaves estrangeiras):

```python
import graphviz

# Inicializa o diagrama
dot = graphviz.Digraph('StarSchema', node_attr={'shape': 'plaintext'})

# Criando a Tabela Fato
dot.node('FatoVendas', '''<
<TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0">
  <TR><TD BGCOLOR="lightgray"><B>Fato_Vendas</B></TD></TR>
  <TR><TD PORT="fk_tempo">id_tempo (FK)</TD></TR>
  <TR><TD PORT="fk_produto">id_produto (FK)</TD></TR>
  <TR><TD>quantidade_vendida</TD></TR>
  <TR><TD>valor_total</TD></TR>
</TABLE>>''')

# Criando uma Tabela Dimensão
dot.node('DimProduto', '''<
<TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0">
  <TR><TD BGCOLOR="lightblue"><B>Dim_Produto</B></TD></TR>
  <TR><TD PORT="pk_produto">id_produto (PK)</TD></TR>
  <TR><TD>nome_produto</TD></TR>
  <TR><TD>categoria</TD></TR>
</TABLE>>''')

# Criando o relacionamento (Join)
dot.edge('FatoVendas:fk_produto', 'DimProduto:pk_produto')

# Renderiza a imagem
dot.render('esquema_estrela', format='png')
