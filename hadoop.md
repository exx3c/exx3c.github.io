<div style="width: 100%; border: 1px solid #dfe2e5; overflow: hidden; margin-bottom: 16px;">
 <div style="width: 100%; background-image: url('https://raw.githubusercontent.com/exx3c/exx3c.github.io/refs/heads/main/DALL·E%202025-01-13%2014.45.38%20-%20A%20formal%20line-art%20style%20drawing%20of%20a%20transformation%20machine%20that%20visually%20represents%20a%20data%20processing%20flow.%20The%20machine%20is%20depicted%20as%20a%20conveyor%20bel.png'); background-size: cover;background-position: center; height: 220px;"></div>
</div>

# ETL com Apache Airflow e Hadoop: Uma Pipeline para Visualizações Interativas de Dados Financeiros

### Índice
1. [Visão Geral](#visão-geral)
2. [Objetivo do Projeto](#objetivo-do-projeto)
3. [Arquitetura do Pipeline](#arquitetura-do-pipeline)
4. [Tecnologias Utilizadas](#tecnologias-utilizadas-e-possibilidades-de-expansão)
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
- **Apache Airflow**: Para orquestração e gerenciamento das pipelines de ETL.
- **Python**: Para desenvolvimento dos scripts de ETL e da DAG.
- **Ferramentas de Visualização**: Qualquer ferramenta capaz de interpretar HTML e CSS.
- **Apache Spark (Possibilidade)**: Para processamento paralelo e distribuído das tarefas conforme a escalabilidade do projeto.
- **Ambiente em Nuvem (Possibilidade)**: AWS MWAA, Elastic Container Service, AWS S3.

## Etapas do Pipeline

### Extração de Dados
A extração de dados foi realizada a partir de uma API financeira ([brapi](https://brapi.dev/)) onde é fornecido o histórico de mercado de Ações Brasileiras.
- Extração inicial dos dados.
- Envio dos dados via cross-communications (XCom).
```python
def get_json():

    # token mascarado com *
    api_token = '***********************'
    url = f'https://brapi.dev/api/quote/PETR4?range=1mo&interval=1d&fundamental=true&token={api_token}'

    try:
        api_response = requests.get(url)

        if api_response.status_code != 200:
            raise ValueError(f'(API) Erro {api_response.status_code}: {api_response.text}')

        else:
            return api_response.text

    except Exception as e:
        raise ValueError(f'(Requests) Erro: {e}')
```
  
### Transformação
O processo inclui:
- Criação e configuração do gráfico usando Plotly.
- Montagem do elemento HTML usando um template como base para inserção.
```python
def create_chart(ti):
    json_data = ti.xcom_pull(task_ids='get_json')
    json_data = json.loads(json_data)

    dates = []
    values = []

    for x in json_data['results'][0]['historicalDataPrice']:
        dates.append(datetime.date.fromtimestamp(x['date']))
        values.append(float(x['close']))

    fig = go.Figure()

    fig.add_trace(go.Scatter(
        x=dates,
        y=values,
        mode='lines+markers',
        name='Valores',
        line=dict(color='steelblue', width=2),
        marker=dict(size=8)
    ))

    fig.update_layout(
        margin=dict(l=10, r=10, t=10, b=10),
        xaxis=dict(fixedrange=True, showgrid=True),
        yaxis=dict(fixedrange=True, tickprefix='R$ ',showgrid=True),
        template="plotly_white",
        autosize=True,
        width=None,
        height=400
    )

    html_element = to_html(fig, full_html=False, config={'displayModeBar': False})

    return BeautifulSoup(html_element, 'html.parser')



def assemble_html(ti):
    html_chart = ti.xcom_pull(task_ids='create_chart')
    json_data = ti.xcom_pull(task_ids='get_json')
    json_data = json.loads(json_data)


    try:
        hdfs_client = InsecureClient('http://127.0.0.1:50070', user='hdfs')
        hdfs_path = f'/stock_charts/template/chart_template.html'

        with hdfs_client.read(hdfs_path, encoding='utf-8') as file:
            html_template = file.read()

        soup = BeautifulSoup(html_template, "html.parser")


        # getting html objects
        price = soup.find(id="priceField")
        change_price = soup.find(id="changePriceField")
        change_percentage = soup.find(id="changePercentageField")
        last_update = soup.find(id="updateInfo")
        opening = soup.find(id="openingField")
        min = soup.find(id="minField")
        max = soup.find(id="maxField")
        chart = soup.find(id="plotlyChart")


        # setting html objects values
        price.string = f'R$ {json_data['results'][0]['regularMarketPrice']}'
        change_price.string = f'R$ {json_data['results'][0]['regularMarketChange']}'
        change_percentage.string = f'{json_data['results'][0]['regularMarketChangePercent']}%'

        date_last_update = datetime.datetime.strptime(str(json_data['results'][0]['regularMarketTime']),
                                             "%Y-%m-%dT%H:%M:%S.%fZ")
        date_last_update = date_last_update.strftime("%d/%m/%Y %H:%M")
        last_update.string = f'Última atualização: {date_last_update}'

        opening.string = f'R$ {json_data['results'][0]['regularMarketOpen']}'
        min.string = f'R$ {json_data['results'][0]['regularMarketDayLow']}'
        max.string = f'R$ {json_data['results'][0]['regularMarketDayHigh']}'

        chart.append(html_chart)


        # setting html objects styles
        if json_data['results'][0]['regularMarketChangePercent'] < 0:
            if 'style' in change_percentage.attrs:
                change_percentage['style'] += 'background-color: #c31b1b !important;'

            else:
                change_percentage['style'] = 'background-color: #c31b1b !important;'

        return str(soup)


    except Exception as e:
        raise ValueError(f'Erro: {e}')
```

### Carregamento (HDFS)
- Upload dos dados brutos recebidos diretamente da API.
- Upload do elemento HTML pronto para uso.
- Carregamento do template HTML (uma única vez).
```python
def upload_json(ti):
    date = datetime.now()
    json_data = ti.xcom_pull(task_ids='get_json')
    file_name = f'PETR4_{date.strftime("%b").lower()}{date.year}.json'

    try:
        hdfs_client = InsecureClient('http://127.0.0.1:50070', user='hdfs')
        hdfs_path = f'/stock_charts/raw/{file_name}'

        with hdfs_client.write(hdfs_path, encoding='utf-8') as writer:
            writer.write(json_data)

        logging.info(f"Arquivo gravado com sucesso em: {hdfs_path}")

    except Exception as e:
        raise ValueError(f'Erro: {e}')



def upload_html(ti):
    date = datetime.now()
    html_data = ti.xcom_pull(task_ids='assemble_html')
    file_name = f'PETR4_{date.strftime("%b").lower()}{date.year}.html'

    try:
        hdfs_client = InsecureClient('http://127.0.0.1:50070', user='hdfs')
        hdfs_path = f'/stock_charts/processed/{file_name}'

        with hdfs_client.write(hdfs_path, encoding='utf-8') as writer:
            writer.write(html_data)

        logging.info(f"Arquivo gravado com sucesso em: {hdfs_path}")

    except Exception as e:
        raise ValueError(f'Erro: {e}')
```


#### Código da DAG (Airflow)
```python
from airflow import DAG
from airflow.operators.empty import EmptyOperator
from datetime import datetime
import utils


default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'email_on_failure': False,
    'email_on_retry': False,
}


with DAG(
    'monthly_dag_example',
    default_args=default_args,
    description='Uma DAG exemplo que roda todo dia 1 de cada mês',
    schedule_interval='0 0 1 * *',  # Cron para o primeiro dia de cada mês às 00:00
    start_date=datetime(2023, 1, 1),
    catchup=False,  # Não executa execuções retroativas
    tags=['example', 'monthly']
) as dag:


    start = EmptyOperator(task_id='start')

    get_json = PythonOperator(
        task_id='get_json',
        python_callable=utils.get_json,
        dag=dag,
    )

    upload_json = PythonOperator(
        task_id='upload_json',
        python_callable=utils.upload_json,
        dag=dag,
    )

    create_chart = PythonOperator(
        task_id='create_chart',
        python_callable=utils.create_chart,
        dag=dag,
    )

    assemble_html = PythonOperator(
        task_id='assemble_html',
        python_callable=utils.assemble_html,
        dag=dag,
    )

    upload_html = PythonOperator(
        task_id='upload_html',
        python_callable=utils.upload_html,
        dag=dag,
    )

    end = EmptyOperator(task_id='end')


    (
        start >> get_json >> upload_json >>
        create_chart >> assemble_html >>
        upload_html >> end
    )
```

## Resultados e Visualizações

Ao final do processo de ETL, os dados são organizados e apresentados em um gráfico interativo gerado pela biblioteca Plotly, que é incorporado em um elemento HTML customizado para poder ser incluso, por exemplo, em uma página de cotação de ações. O gráfico exemplificado acima é um dos resultados finais gerados pela pipeline.

![image](https://github.com/user-attachments/assets/a1b37621-1847-4056-a449-9ccbb5db3f37)


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
