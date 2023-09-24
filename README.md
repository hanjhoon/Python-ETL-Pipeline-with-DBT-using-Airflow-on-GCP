# Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP

# Google Cloud Platform을 사용하여 Airflow에서 Astro, DBT, Soda 및 Metabase를 사용하여 Python 데이터 파이프라인을 조율해보자

이 프로젝트는 Airflow의 DAG(Directed Acyclic Graphs)를 사용하여 ETL 파이프라인을 빌드하고 자동화하고 변환된 데이터를 BigQuery로로드하는 방법을 보여줍니다. 이 프로젝트에서는 Astro (Airflow를 둘러싼 도커 래퍼), DBT (데이터 모델링 및 SQL을 사용한 보고서 생성), Soda (데이터 품질 확인에 사용) 및 Google Cloud Platform (테이블 저장에 사용)과 같은 다양한 도구를 사용합니다.

# 프로젝트 목표 - 새로운 도구 사용 및 학습

1. 데이터 수집 - GCP BigQuery로 새로운 데이터를 추출하는 데이터 수집 파이프라인 생성.

2. 데이터 품질 - YAML 파일을 사용하여 맞춤형 데이터 품질 확인을 수행하는 Soda 사용.

3. 데이터 변환 - DBT를 사용하여 데이터 모델링 수행하고 데이터를 스타 스키마로 변환.

4. 데이터 로딩 - 추출 및 변환된 데이터를 GCP BigQuery로 로드하는 데이터 파이프라인 사용.

5. 데이터 보고/분석 - 보고서 또는 분석 목적으로 대시보드를 만들기 위해 Metabase 사용.

# Data Architecture

![image](https://github.com/hanjhoon/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/121271030/930583b9-d773-46ab-9968-6afd404b9430)

# 사용된 데이터 세트
온라인 리테일 II 데이터 세트에는 2009년 12월 1일부터 2011년 12월 9일까지 영국 기반 및 등록 상점이 아닌 온라인 리테일의 모든 거래가 포함되어 있습니다. 이 회사는 주로 고유한 모든 행사용 기프트웨어를 판매합니다. 회사의 많은 고객은 도매상입니다.

Dataset link: [Online Retail Dataset](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci)

# 이 프로젝트에서 사용된 도구 및 기술
1. BigQuery (GCP) - BigQuery는 Google Cloud에서 제공하는 완전히 관리되는 서버리스 데이터 웨어하우스 및 분석 플랫폼입니다. 대용량 데이터 집합을 확장 가능하고 비용 효율적인 방식으로 저장하고 분석하기 위해 설계되었습니다. BigQuery는 Google Cloud Platform (GCP)의 일부이며 비즈니스 인텔리전스, 데이터 웨어하우징, 데이터 탐색 및 기계 학습을 포함한 다양한 데이터 처리 및 분석 작업에 널리 사용됩니다.
2. SODA - SODA (Scalable Open Data Automation)는 대규모 데이터 집합의 유효성 및 품질 확인을 자동화하기 위해 설계된 데이터 품질 프레임워크 및 도구입니다. 주로 데이터의 품질, 무결성 및 규정 준수를 확인하는 데 사용됩니다. SODA는 특정 프로그래밍 언어나 기술 스택에 특정하지 않으며 다양한 데이터 원본 및 형식에 적용할 수 있습니다.
3. DBT - dbt (데이터 빌드 도구)는 현대 데이터 분석 및 엔지니어링을 위한 오픈 소스 명령 줄 도구 및 모델링 프레임워크입니다. 데이터 분석가 및 데이터 엔지니어가 데이터 웨어하우스 내에서 데이터의 변환과 모델링을 관리하는 데 도움을 주도록 설계되었습니다. dbt는 데이터를 버전 관리 가능하고 테스트 가능하며 유지 관리 가능한 방식으로 변환 및 모델링을 지원하기 위해 소프트웨어 개발 최적 사례와 유사한 방식으로 중점을 둡니다.
4. Astro CLI - Astro CLI는 데이터 조율을위한 명령 줄 인터페이스입니다. 이것은 Astronomer 스위트의 일부이며 Apache Airflow를보다 쉽게 시작할 수 있도록 도와주며 모든 Astronomer 제품과 함께 사용할 수 있습니다. Astro 프로젝트에는 Airflow를 실행하는 데 필요한 파일 세트가 포함되며 DAG 파일, 플러그인 및 종속성에 대한 전용 폴더가 포함됩니다.
5. Metabase - Metabase는 비즈니스 인텔리전스 (BI) 및 데이터 분석 도구로 조직이 데이터를 쉽게 시각화하고 분석할 수 있도록 하는 오픈 소스 도구입니다. 이는 차트, 대시 보드 및 보고서를 생성하기 위한 사용자 친화적 인 인터페이스를 제공하며 깊은 기술 또는 SQL 지식이 필요하지 않습니다. Metabase는 데이터 탐색 및 보고서 작성을 조직 내의 다양한 사용자에게 액세스 가능하게 만들도록 설계되었으며 비즈니스 분석가, 데이터 분석가 및 비기술 스테이크 홀더를 포함한 조직 내 다양한 사용자에게 데이터 탐색 및 보고서 생성을 가능하게합니다.
6. 비주얼 스튜디오 코드 - Visual Studio Code, 일반적으로 VS Code로 줄여쓰며, Microsoft에서 개발한 무료 오픈 소스 코드 편집기입니다. 개발자 사이에서 가장 인기있는 코드 편집기 중 하나가되어 다양한 프로그래밍 언어 및 플랫폼에서 코드 작성 및 편집에 널리 사용됩니다. VS Code는 유연성, 폭넓은 확장 마켓 플레이스 및 다양한 기능으로 알려져 있으며 다양한 개발 작업에 적합합니다.
7. 도커 - 도커는 개발자가 응용 프로그램과 의존성을 가볍게 패키지화하고 배포할 수 있게 해주는 플랫폼 및 기술입니다. 이러한 컨테이너는 응용 프로그램 및 해당 라이브러리, 의존성 및 구성을 실행하는 데 필요한 모든 것을 캡슐화하는 격리된 환경입니다. 도커 컨테이너는 일관성과 이식성을 제공하여 응용 프로그램을 개발, 배포 및 다양한 환경 (개발, 테스트 및 프로덕션)에서 실행하기 쉽게 만듭니다.
8. Git 버전 제어 - Git은 소프트웨어 개발 및 기타 협업 프로젝트에서 소스 코드, 문서 및 기타 유형의 파일의 변경 내용을 추적하기 위해 널리 사용되는 분산 버전 제어 시스템 (VCS)입니다. Git과 같은 버전 제어 시스템은 프로젝트 파일의 변경 내용을 관리하고 소프트웨어 개발 및 기타 창의적인 작업에 대한 협업을 효과적으로 관리하는 데 개발자 및 팀에 도움이 됩니다.


# Implementation

* **Step 1** - Astro CLI를 설치하고 GCP 연결이 포함된 작업 Airflow 환경을 생성합니다.
Astro CLI는 로컬에 Airflow 환경을 설정하는 데 도움이되는 도커 래퍼와 같은 것으로 생각할 수 있으며 Airflow의 웹 서버, 스케줄러, 데이터베이스 및 트리거와 같은 모든 구성 요소를 손쉽게 설정할 수 있습니다. 이는 Airflow 서버를 만드는 컨테이너화 된 방법입니다. 이 단계를 수행하려면 시스템에 하이퍼-V가 활성화되어 있는 Docker가 설치되어 있어야합니다. Astro CLI를 설치한 후, 빈 디렉토리에서 다음 명령을 실행하십시오.

  ```
  astro dev init
  ```

이 명령은 디렉토리에 Airflow 구성 요소의 구조를 만들며 이전에 수동으로 파일을 만들었던 것처럼 Airflow 구성 요소를 만들어냅니다. 프로젝트 폴더 구조는 다음과 같습니다.

![image](https://github.com/hanjhoon/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/121271030/1819cc38-1736-4f74-8eb5-d17492310c7e)

이 단계 후, Google Cloud에 무료 티어로 계정을 만들고 프로젝트를 만듭니다. 프로젝트 ID를 메모해 두세요. 나중에 여러 곳에서 사용하게 됩니다. Google Cloud와 Airflow 사이에서 연결을 설정하기 위해 Google Cloud IAM 설정에서 "서비스 계정"이라는 것을 사용합니다. 연결은 Google Cloud Storage와 Airflow 사이에서 이루어집니다. 따라서 저장소 관리자 계정과 BigQuery 관리자 계정을 서비스 계정 설정에서 만듭니다. 키를 다운로드하고 "gcp" 폴더 아래의 "include" 폴더에 넣으세요. 다른 사람과 공유하지 마십시오.

연결은 양방향입니다

마치 Google이 Airflow와 연결해야하는 것처럼, 반대로도 그렇습니다. Airflow에서 "공급자"라는 커뮤니티가 있으며 이를 사용하여 Airflow 핵심 위에 추가 기능을 만들고 다양한 기술을 사용하여 소프트웨어와 상호 작용할 수 있습니다. 따라서 우리의 경우 Google 공급자를 설치합니다.


```
apache-airflow-providers-google
```

위 명령을 requirements.txt 파일에 추가한 후 도커 컨테이너를 시작할 수 있습니다.

```
astro dev start
```

모든 것이 잘 작동하는 경우 localhost:8080에서 Airflow UI에 액세스 할 수 있으며 여기서 서비스 계정 자격 증명을 사용하여 GCP 연결을 만듭니다. Airflow "include" 디렉토리 아래의 "gcp" 폴더에 서비스 계정 자격 증명을 저장해야합니다.


* **Step 2** - Airflow에서 DAG를 사용하여 데이터 파이프 라인을 생성합니다.
DAG (Directed Acyclic Graph)는 핵심 개념이자 기본적인 구성 요소이며 실행할 작업의 워크플로우 또는 순차적으로 실행할 작업의 시퀀스를 나타냅니다. 여기서 각 작업은 작업 단위이며 작업 간의 엣지는 실행 순서를 정의합니다.

    * Task1:
          여기에서 연산자는 Airflow에서 다양한 기술에 대한 작업을 실행하기위한 사용자 정의 코드를 작성하는 연결 방법입니다. 연산자는 각 작업에서 실행할 작업을 정의하며 Python 스크립트 실행과 같은 간단한 작업부터 시스템 간 데이터 전송 또는 SQL 쿼리 실행과 같은 복잡한 작업까지 다양할 수 있습니다. 연산자는 특정 작업에 필요한 로직 및 매개 변수를 캡슐화하는 Python 클래스입니다.

    ```
    from airflow.decorators import (
    dag,
    task,)
    
    # DAG and task decorators for interfacing with the TaskFlow API
    from datetime import datetime
    from airflow.providers.google.cloud.transfers.local_to_gcs import LocalFilesystemToGCSOperator
    from airflow.providers.google.cloud.operators.bigquery import BigQueryCreateEmptyDatasetOperator
    
    upload_csv_to_gcs = LocalFilesystemToGCSOperator(
    task_id='upload_csv_to_gcs',
    src='/usr/local/airflow/include/dataset/online_retail.csv',
    dst='raw/online_retail.csv',
    bucket='online_retail_database',
    gcp_conn_id='gcp',
    mime_type='text/csv')
    ```


     <p> </p> It's always good to testa task after creating it and so we can do that by running the following command:</p>


    ```
    astro dev bash -> airflow tasks test <DAG name> <Task Name> <Start Date>
    ```
    
    이렇게 하면 Google Cloud Storage 버킷에 파일을 저장하고 해당 DAG ID와 작업 ID와 함께 작업을 성공으로 표시해야합니다.

    ![image](https://github.com/hanjhoon/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/121271030/8b864508-1c26-476f-9298-c8c64e8316fa)


    * Task2:
      BigQuery에서 SQL 테이블을 생성하려면 "데이터셋"이 있어야합니다. 데이터셋 내에서 관계형 테이블을 배치할 수 있습니다. 이 작업은 BigQuery 내에 데이터셋을 만듭니다.
      
      ```
      create_retail_dataset = BigQueryCreateEmptyDatasetOperator(
      task_id='create_retail_dataset',
      dataset_id='retail',
      gcp_conn_id='gcp',)
      ```

    ![image](https://github.com/hanjhoon/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/121271030/bd2f5918-f07a-4201-ab40-185d26ebdde0)


    * Task3:
      파일을 BigQuery의 테이블에 저장하는 load file 연산자를 사용합니다.

      ```
      gcs_to_raw = aql.load_file(
  
          task_id='gcs_to_raw',
          input_file=File(
              'gs://online_retail_database/raw/online_retail.csv',
              conn_id='gcp',
              filetype=FileType.CSV,
          ),
          output_table=Table(
              name='raw_online_retail',
              conn_id='gcp',
              metadata=Metadata(schema='retail')
          ),
          use_native_support=False,      )
      ```

    ![image](https://github.com/hanjhoon/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/121271030/be4860c8-601a-4f0d-9b12-50a917dd3d56)


* **Step 3** - 테스트 및 실행
DAG를 정의하고 모든 작업을 만든 후 DAG를 테스트하고 실행할 수 있습니다. 로컬 Airflow 환경에서 DAG를 수동으로 실행하려면 다음 명령을 사용하십시오.

    ![image](https://github.com/hanjhoon/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/121271030/bf838e42-3b85-413b-9d0f-342acf15dba9)

```
astro dev bash   
```


```
soda scan -d retail -c include/soda/configuration.yml include/soda/checks/sources/raw_online_retail.yml
```

그러나 검사를 수정하면 예를 들어 숫자 열의 데이터 유형을 문자열로 의도적으로 변경하면 오류가 발생합니다. 이제 DAG 내에서 데이터 품질 검사를 만들기 위해 Python 외부 작업을 사용할 것입니다. 이 작업을 사용하면 DAG 내에서 외부 가상 환경에서 실행되는 모든 Python 코드를 실행할 수 있으므로 다른 오픈 소스 도구와의 의존성 충돌을 방지할 수 있습니다. Python 가상 환경을 만들고 활성화하기 위해 Dockerfile을 다음과 같이 수정할 것입니다.

```
RUN python -m venv soda_venv && source soda_venv/bin/activate && \
    pip install --no-cache-dir soda-core-bigquery==3.0.45 &&\
    pip install --no-cache-dir soda-core-scientific==3.0.45 && deactivate
```

DAG 내에서 Python 외부 작업을 사용하면 DAG 내에서 외부 가상 환경에서 실행되는 Python 코드를 실행할 수 있습니다. 이를 통해 Google BigQuery와 상호 작용하기 위해 해당 라이브러리와 종속성을 포함한 soda 라이브러리를 사용할 수 있습니다. 환경을 활성화하고 라이브러리를 설치한 다음 환경을 비활성화합니다.

```
@task.external_python(python='/usr/local/airflow/soda_venv/bin/python')
def check_load(scan_name='check_load', checks_subpath='sources'):
    from include.soda.check_function import check

    return check(scan_name, check_subpath)

check_load()
```

DAG 내에서 실행되는 Python 환경에서 호출될 'check_function'은 저장소 파일에 있습니다. DAG 내에서 데코레이터를 사용할 때는 해당 작업을 DAG 내에서 명시적으로 호출해야 합니다.


* **Step 4** - DBT 및 Soda 연동

DBT와 Soda는 Python DAG 내에서 통합될 수 있습니다. DBT를 사용하여 데이터를 모델링하고 Soda를 사용하여 데이터의 품질을 확인하는 방법은 다음과 같습니다.

  이것은 실제로 사용되는 스타 스키마 데이터 모델링입니다. 여기서는 사실(fact) 테이블이 차원(dimension) 테이블로 둘러싸여 있습니다. 사실 테이블에는 데이터베이스의 모든 숫자 및 키가 포함되며, 차원 테이블에는 제품, 고객, 날짜 및 기타와 같은 맥락 및 배경 정보가 포함되어 있습니다.

![image](https://github.com/hanjhoon/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/121271030/61135f3b-6981-4ae2-96f2-ea4a1ec53c77)


Airflow와 함께 dbt를 통합하기 위해 Cosmos를 사용하며, Cosmos를 통해 dbt 모델 내부의 프로세스에 대한 더 많은 정보를 얻을 수 있습니다. 각 모델은 DAG 내에서 작업이 되어 더 나은 관찰성을 제공합니다. Cosmos는 dbt 모델을 가상 Python 환경 내에서 실행하므로 동일한 작업을 위해 새로운 가상 환경을 만듭니다.

  ```
  RUN python -m venv dbt_venv && source dbt_venv/bin/activate && \
    pip install --no-cache-dir dbt-bigquery==1.5.3 && deactivate
  ```
  All the profile.yml, dbt_project.yml, sources.yml and packages.yml files are uploaded in files section. All the .sql files are also uploaded. After running the dbt models using dbt cli, we can see four tables inside bigquery:

  <p align="center">
  <img width = "700" height="230" src="https://github.com/chayansraj/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/22219089/2cdeb95e-cb9c-4cab-bfd4-5f44ee3341c8">
  <h6 align = "center" > Source: Author </h6>
  </p>

  이제 Airflow DAG 내에서 dbt 모델을 통합하여 Airflow가 해당 작업을 실행할 수 있습니다.
  
  ```
  transform = DbtTaskGroup(
    group_id = 'transform',
    project_config = DBT_PROJECT_CONFIG,
    profile_config = DBT_CONFIG,
    render_config = RenderConfig(
        load_method = LoadMode.DBT_LS,
        select=['path:models/transform']
    ))

```
이 작업을 구현하면 Airflow UI에서 DAG를 검사하여 아래에 나와 있는 것과 같이 더 상세하게 볼 수 있습니다.

  ![image](https://github.com/hanjhoon/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/121271030/8d1d0625-4b13-4f98-8f74-1877f18faff2)

  Similarly, we can implement the same for reporting models using sql and dbt. 

* **Step 5** - 단계 3을 반복하여 데이터를 사실(fact) 및 차원(dimension) 테이블로 변환한 후 데이터 품질 검사를 실행합니다. 변환 작업용 품질 검사 파일은 파일 섹션에 제공됩니다. DAG에 변환된 테이블을 확인할 추가 작업을 만듭니다.

```
@task.external_python(python='/usr/local/airflow/soda_venv/bin/python')
def check_transform(scan_name='check_transform', checks_subpath='transform'):
    from include.soda.check_function import check

    return check(scan_name, checks_subpath)
```

해당 'check_function'은 파이썬 환경에서 실행되며 DAG에서 호출될 것입니다. 이제 Airflow에서 순차적으로 수행할 작업의 체인을 생성하는 시간입니다. 이것은 다음과 같이 DAG 내에서 chain 메서드를 사용하여 수행됩니다.

```
chain(
    upload_csv_to_gcs,
    create_retail_dataset,
    gcs_to_raw,
    check_load(),
    transform,
    check_transform(),
    report,
    check_report(),
)
```

  ![image](https://github.com/hanjhoon/Python-ETL-Pipeline-with-DBT-using-Airflow-on-GCP/assets/121271030/24c30ca8-978f-4f68-ae33-e038353c2c76)



* **Step 6** -Metabase를 사용하여 대시보드를 만들고 있으며 Metabase는 오픈 소스 데이터 시각화 및 분석 플랫폼입니다. 이를 로컬 머신에서 실행하며 로컬호스트(localhost)에서 실행됩니다. Metabase를 실행하기 위해 아래와 같은 Docker Compose 오버라이드 파일을 생성합니다.


  ```
  version: "3.1"
  services:
  metabase:
    image: metabase/metabase:v0.46.6.4
    volumes:
      - ./include/metabase-data:/metabase-data
    environment:
      - MB_DB_FILE=/metabase-data/metabase.db
    ports:
      - 3000:3000
    restart: always
  ```

  
We restart the airflow instance and we can start accessing metabase from localhost:3000. 
Using the tables above and performing data analytics, we can finally create a dashboard.
