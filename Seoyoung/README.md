## local 작업 환경 셋팅
크게 아래와 같은 두 환경으로 분리했습니다.

A. jupyter notebook을 사용하여 interactive하게 pyspark를 사용한 데이터 처리 코드를 개발할 수 있는 환경
    - Dockerfile, docker-compose.yml

B. master 1, worker 1 로 구성된 spark cluster 환경
    - #TODO, docker-compose_prod_test.yml

#### A, B 공통 구성
- aws 관련 구성
    - aws/ 경로 하의 config ini파일
    - spark-default.conf
        - s3a 사용
            - s3a: s3와 상호작용하기 위해 hadoop 에서 제공하는 파일시스템 인터페이스. (use object storage like its file system)
            - 이를 사용해 hdfs 대신 s3를 파일시스템(DataLake)으로 사용.(의견 필요)
            - data write command eg) `hadoop fs -put file.txt s3a://bucket_name/file.txt`
                
- python libraries
    - requirements.txt로 관리
    - 기본 필요 라이브러리 현재 `awscli`, `boto3`, `pandas`, `pytest` 로 정의


## Getting Started
1. aws/.env_template 파일에 aws 자격증명 명시, _template posfix 제거
2. conf/spark-defaults_template 파일에 aws 자격증명 명시, _template posfix 제거
3. 로컬 개발 환경 구성
    3-1. local mode 환경(A) `docker-compose up -d`
    3-2. spark standalone cluster 환경(B) `docker-compose -f docker-compose_prod_test.yml up -d`