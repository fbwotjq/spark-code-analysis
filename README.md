# Spark 소스 코드 분석

본 주제는 크게 두 장으로 나누어져 있는데, 첫 번째 장은 스파크 코어의 소스코드 분석에 관한 것이고, 두 번째 장은 스파크 SQL의 소스코드 분석에 관한 것이다.

## spark core

본 장에서는 주로 컴퓨팅과 스토리지라는 두 가지 관점에서 스파크 코어의 소스 코드를 분석합니다.

### computing
computing에 관련된 내용: 작업 및 작업의 예약 및 shuffle이 포함됩니다. 여기서는 독자가 이미 RDD의 개념과 원리를 이해하고 있다고 가정하므로 RDD는 내용에 포함되지 않습니다.

[스파크 기본 사항][1]
 - 이 섹션에서는 주로 Job, Stage, Task, 종속성 등과 같은 개념을 소개하고 이러한 개념 간의 관계를 설명하기 위한 간단한 예부터 시작합니다. 주로 Job의 스케줄링을 분석합니다. 마지막으로 Shuffle 프로세스와 관련된 개념과 과정을 간략하게 소개합니다.

[Spark의 셔플 메커니즘][2]
 - 이 섹션에서는 주로 Spark의 다양한 Shuffle 구현 프로세스를 분석합니다.

[Spark의 작업 스케줄링 메커니즘][3]
 - 이 섹션에서는 핵심 개념과 구현 프로세스를 포함하여 작업 실행 계획(예약) 메커니즘을 소개합니다.

### 저장 (persist)

스토리지 관리는 두 섹션으로 나누어 설명하는데, 첫 번째 섹션은 사용자 관점, 즉 데이터 저장 단위 Block의 관점에서 설명하고, 두 번째 섹션은 구현 관점, 즉 메모리 관리 관점에서 설명한다.

[Spark Block관리][4]
 - 저장 처리와 관련된 많은 작업(예: Shuffle、broadcast 등)，모두 BlockManager를 포함합니다. 이 섹션에서는 주로 BlockManager 및 관련 개념을 분석합니다.

[스파크 메모리 관리][5]
 - BlockManager는 MemoryManager를 이용하여 특정 저장소(메모리, 디스크 등)를 구현합니다. 따라서 이 섹션에서는 Spark의 메모리 관리 구현을 분석합니다.
 
## spark sql

본 장은 Catalyst 분석 부분과 Join 상세 설명 부분으로 나누어져 있습니다. Catalyst 분석 부분은 SQL 문장의 파싱부터 logical plan까지의 분석을 포함하고, Join 상세 설명 부분은 Join 연산자에 대한 분석을 포함합니다.

### Catalyst 분석
Catalyst 분석 부분은 SQL 문장의 파싱부터 logical plan까지의 분석을 포함하고, Join 상세 설명 부분은 Join 연산자에 대한 분석을 포함합니다.

![Catalyst 구현][Catalyst]

[Spark SQL 기본 사항][7]
이 섹션에서는 Row, Expression, Attribute, QueryPlan 및 Tree를 포함하여 Spark와 관련된 몇 가지 주요 개념을 주로 소개합니다.

[Spark Catalyst 분석 단계][6]
 - 이 섹션은 분석을 위한 SQL 문으로 시작됩니다. 이 섹션은 SQL 문을 구문 분석하고 LogicalPlan 내용를 생성하는 작업이 포함됩니다.

[Catalyst Logical Plan 최적화][8]
 - 이 섹션에서는 이전 섹션을 기반으로 LogicalPlan을 최적화하고 주로 일부 최적화 솔루션을 분석합니다.

[Spark SQL Physical Plan][9]
 - 최적화된 Logical Plan을 처리하고 실행을 위한 Physical Plan을 생성하는 섹션입니다.

[Spark SQL 실행 단계][10]
 - PhysicalPlan이 생성된 후 실행 단계로, 실행 과정에서 몇 가지 최적화 계획이 포함되며, 이 섹션에서는 DataSet의 원리를 분석합니다.

### Join
이 부분은 주로 Join 연산자에 대한 분석을 위한 부분으로, 전반부 분석에서는 기본적인 처리 아이디어를 다루지만, 모든 연산자를 분석하기에는 충분히 자세하지 않습니다. 전반전 라인.

[Spark SQL Join 분석 - 1부][11]
 - 특정 Join 연산자의 경우 SQL 문의 AST 트리 구문 분석부터 LogicalPlan 최적화 완료, 즉 PhysicalPlan이 생성되기 전의 코드를 분석합니다.

[Spark SQL Join 분석 - 2부][12]
 - 특정 Join 연산자의 경우 LogicalPlan에서 Spark 코어의 특정 셔플 작업에 이르기까지 이전에 분석을 수행했습니다.

[1]:https://github.com/fbwotjq/spark-code-analysis/blob/master/analysis/core/spark_shuffle.md
[2]:https://github.com/fbwotjq/spark-code-analysis/blob/master/analysis/core/spark_sort_shuffle.md
[3]:https://github.com/fbwotjq/spark-code-analysis/blob/master/analysis/core/task_schedule.md
[4]:https://github.com/fbwotjq/spark-code-analysis/blob/master/analysis/core/block_manager.md
[5]:https://github.com/fbwotjq/spark-code-analysis/blob/master/analysis/core/memory_manager.md
[6]:https://github.com/fbwotjq/spark-code-analysis/blob/master/analysis/sql/spark_sql_parser.md
[7]:https://github.com/fbwotjq/spark-code-analysis/blob/master/analysis/sql/spark_sql_preparation.md
[8]:https://github.com/summerDG/spark-code-analysis/blob/master/analysis/sql/spark_sql_optimize.md
[9]:https://github.com/summerDG/spark-code-analysis/blob/master/analysis/sql/spark_sql_physicalplan.md
[10]:https://github.com/summerDG/spark-code-analysis/blob/master/analysis/sql/spark_sql_execution.md
[11]:https://github.com/summerDG/spark-code-analysis/blob/master/analysis/sql/spark_sql_join_1.md
[12]:https://github.com/summerDG/spark-code-analysis/blob/master/analysis/sql/spark_sql_join_2.md
[Catalyst]:pic/Catalyst-Optimizer-diagram.png
