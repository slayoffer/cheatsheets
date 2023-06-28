### Повторяющиеся правила

```yaml
.tests: # "Скрытая джоба". В ней мы указываем те директивы, которые будут повторяться. Например, какой-то скрипт. Или only, как здесь
  script: rake test
  stage: test
  only:
    refs:
      - branches

rspec: 
  extends: .tests # Данная строчка говорит гитлабу: возьми все директивы, что есть в джобе .tests, кроме тех, что указаны в джобе rspec
  image: ubuntu:focal
  only:
    variables:
      - $RSPEC
```

Без extends джобу rspec можно будет описать вот так:

```yaml
rspec:
  script: rake test
  stage: test
  image: ubuntu:focal
  only:
    refs:
      - branches
    variables:
      - $RSPEC
```

### Якоря

```yaml
.job_template: &job_configuration  # Задаем "скрытую джобу", давая ей алиас job_configuration  
  image: ruby:2.6
  services:
    - postgres
    - redis

test1:
  <<: *job_configuration  # Берем все содержимое скрытой джобы с алиасом job_configuration и склеиваем с заданными директивами в джобе test1
  script:
    - test1 project

test2:
  <<: *job_configuration  # Берем все содержимое скрытой джобы с алиасом job_configuration и склеиваем с заданными директивами в джобе test2
  script:
    - test2 project
```

Extends и якори по сути решают одну и ту же проблему. Предпочтительнее использовать первое, поскольку данный функционал более гибкий

### Global config

Если мы укажем какую-то директиву не на уровне джобы, а на верхнем уровне, то она применится ко всем джобам. Пример:

```yaml
image: ubuntu:latest #image будет использован для всех джобов, кроме тех, где этот параметр переопределен
my-job:
  image: ubuntu:focal #Здесь мы используем директиву на уровне джобы, соответственно, перекроем то, что выше
  stage: build

my-job-2:
  stage: build
```