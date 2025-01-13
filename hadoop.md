<div style="width: 100%; border: 1px solid #dfe2e5; overflow: hidden; margin-bottom: 16px;">
 <div style="width: 100%; background-image: url('https://raw.githubusercontent.com/exx3c/exx3c.github.io/refs/heads/main/leetcode_1.png'); background-size: cover;background-position: center; height: 220px;"></div>
</div>

# ETL com Apache Airflow e Hadoop: Uma Pipeline para Visualizações Interativas de Dados Financeiros

### Índice
1. [Visão Geral](#visão-geral)
2. [Objetivo do Projeto](#objetivo-do-projeto)
3. [Arquitetura do Pipeline](#arquitetura-do-pipeline)
4. [Tecnologias Utilizadas](#tecnologias-utilizadas)
5. [Etapas do Pipeline](#etapas-do-pipeline)
   - [Extração de Dados](#extração-de-dados)
   - [Transformação de Dados](#transformação-de-dados)
   - [Carregamento de Dados](#carregamento-de-dados)
6. [Resultados e Visualizações](#resultados-e-visualizações)
7. [Conclusão](#conclusão)

---

## Visão Geral
Este projeto apresenta a construção de uma pipeline de ETL (Extração, Transformação e Carregamento) para integrar e processar dados financeiros, com o objetivo de automatizar a geração de visualizações interativas em HTML. A proposta central é coletar mensalmente dados de ações da **PETR4** a partir de uma API financeira ([brapi](https://brapi.dev/)), processá-los e transformá-los em um elemento HTML pronto para ser utilizado em um site, blog ou página de resumo.

## Objetivo do Projeto
O principal objetivo deste projeto é demonstrar o uso de ferramentas de orquestração e armazenamento de dados no gerenciamento de pipelines ETL (Extração, Transformação e Carregamento).
Mais do que apenas implementar uma solução técnica, este projeto busca explorar a essência do processo ETL, destacando que ele não se limita a dados estruturados ou cálculos matemáticos. A essência do ETL está no ciclo completo que os dados percorrem: desde sua entrada como informação bruta, passando por uma transformação que os contextualiza e agrega valor, até sua saída como um novo objeto com uma função específica. Este processo reflete o propósito principal de pipelines de dados — a geração de valor a partir de informações dispersas e não refinadas.

## Arquitetura do Pipeline
O pipeline foi implementada em quatro etapas principais: **Extração**, **Carregamento**, **Transformação** e novamente **Carregamento**.

![Diagrama sem nome drawio (1)](https://github.com/user-attachments/assets/3161fce8-2f5d-495f-a9a6-94d0078ba185)

## Tecnologias Utilizadas e Possibilidades de Expansão
- **Hadoop**: Para o armazenamento distribuído de dados (HDFS).
- **Apache Spark (Possibilidade)**: Para processamento paralelo e distribuído das tarefas conforme a escalabilidade do projeto.
- **Python**: Para desenvolvimento dos scripts de ETL e da DAG.
- **Ferramentas de Visualização**: Qualquer ferramenta capaz de interpretar HTML e CSS.
- **Ambiente em Nuvem (Possibilidade)**: AWS MWAA, Elastic Container Service, AWS S3.

## Etapas do Pipeline

### Extração de Dados
A extração de dados foi realizada a partir de [descreva a fonte de dados, e.g., APIs, arquivos CSV, logs do sistema]. O processo inclui:
- Extração inicial dos dados e armazenamento bruto no HDFS.
- [Adicione detalhes sobre o tipo e o volume de dados extraídos.]

#### Código de Exemplo
```python
# Exemplo de extração de dados com Python
import requests
# Código para extrair dados da API e armazenar no HDFS
```

## Resultados e Visualizações

asdasdasdasd

# Conclusão

A pipeline desenvolvida automatiza o processo de criação de elementos HTML personalizados, economizando tempo e garantindo consistência no design e apresentação das informações. Essa abordagem é ideal para sites que demandam atualizações frequentes, como portfólios financeiros, painéis de controle ou páginas de resumo de investimentos.

Com este projeto, é possivel demonstrar habilidades com pipeline de dados (ETL), integração com APIs, ferramentas como Hadoop e Apache Airflow, manipulação de dados e uso de ferramentas de visualização como o Plotly, além de competências na geração de códigos HTML prontos para integração em sistemas web.

## Agradecimentos e Contato

Obrigado pela sua atenção! Caso queira enviar sugestões ou apenas dizer olá, fique à vontade para me contatar!

- **Email:** [gabriel.dultra.239@gmail.com](mailto:gabriel.dultra.239@gmail.com)
- **LinkedIn:** [gabriel-dultra](https://www.linkedin.com/in/gabriel-dultra/)
- **GitHub:** [exx3c](https://github.com/exx3c/)

---

© [Gabriel Dultra] - Todos os direitos reservados.
