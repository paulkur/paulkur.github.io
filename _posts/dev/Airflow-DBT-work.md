# Airflow DBT work
- [Connections in Airflow](#Connections-in-Airflow)
- [Airflow cmd and from tuts](#Airflow)
- [DBT cmd and from tuts](#DBT)
- [Aliases](#Aliases)
***
Clone repo

    gcl https://github.com/amugnier/3m_onchain_analysis.git
cd to repo dir `3m_onchain_analysis`. Start docker `dcs`. Check if portainer running `dc ps`. Open VScode in repo `code .` Start airflow `start`. cd in airflow container `getair`. After done, open in browser
localhost:
* [Portainer](http://localhost:9000)
* [Airflow](http://localhost:8080)
* [PGadmin](http://localhost:5050)

remote dev pipeline:
* [Portainer](http://paulkur.duckdns.org:4429)
* [Airflow](http://paulkur.duckdns.org:8080)
* [PGadmin](http://paulkur.duckdns.org:5050)
* [webmin](https://paulkur.duckdns.org:4430)

## Connections in Airflow
postgres DB

    Connection Id = db_postgres
    Connection Type = postgres
    Description = POSTGRES connection to DB
    Host = postgres
    Login = airflow
    Pass = airflow
    Port = 5432
API

    Connection Id = user_api
    Connection Type = http
    Description = API for getting users
    Host = https://randomuser.me/
API coingeco

    Connection Id = coin_gecko_api
    Connection Type = http
    Description = API for coingeco
    Host = https://api.coingecko.com/api/v3
API messari

    Connection Id = messari_api
    Connection Type = http
    Description = API for messari
    Host = https://data.messari.io/api/v2
structure: https://data.messari.io/api/v2/assets?fields=id,slug,symbol,metrics/market_data/price_usd

API coinapi.io

    Connection Id = coin_api_ohlcv
    Connection Type = http
    Description = API for coinapi.io
    Host = https://rest.coinapi.io/v1
API gorest.co.in

    Connection Id = api_posts 
    Connection Type = http
    Description = API for getting posts
    Host = https://gorest.co.in/public/v2
[Back to List](#Airflow-DBT-work)
***
## Airflow 
Connect to airflow docker `getair`. Testing commands

    airflow tasks test [DAG_ID] [task_ID] [time_in_past] 

    airflow tasks test coin_api_test is_coin_api_active 2022-3-1
    airflow tasks test messari_api is_mess_api_active 2022-3-1

    airflow tasks test api_test is_api_active 2022-3-1
    airflow tasks test api_test get_posts 2022-3-1
    airflow tasks test api_test save_posts 2022-3-1

    airflow tasks test user_processing creating_table 2020-01-01
    airflow tasks test user_processing is_api_available 2020-01-01
    airflow tasks test user_processing extracting_user 2020-01-01
    airflow tasks test user_processing processing_user 2020-01-01
    airflow tasks test user_processing storing_user 2020-01-01

    airflow tasks test coin_api_test get_ohlcv 2022-3-1
    airflow tasks test coin_api_test save_ohlcv 2022-3-1

    airflow tasks test CG_price_history_data_pipeline is_coingecko_available 2022-5-1

other cmd list

    airflow db init
    airflow db upgrade
    airflow db reset
    airflow webserver
    airflow scheduler
    airflow dags list
    airflow tasks list
    airflow tasks list example_xcom_args_with_operators
    airflow dags trigger -e 2020-01-01 example_xcom_args_with_operators
## Airflow cmd from tuts
Setup Airflow

    mkdir ~/repos/demo/airflow && cd airflow
    curl -o get_pip.py https://bootstrap.pypa.io/get-pip.py
    export PATH=/home/paul/.local/bin:$PATH
    python3 ./get_pip.py
    pip install virtualenv
    virtualenv env
    aenv
    which python
    export AIRFLOW_HOME=~/airflow
    pip install pendulum
    pip3 install poetry
    pip install "apache-airflow==2.4.3" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.5.0/constraints-3.10.txt"
    pip list --format=columns
Note: To stop the Airflow webserver, go to the Airflow terminal and press “ctrl” + “c”. But don’t do this yet!

## DBT
cmd list

    dbt debug
    dbt test
    dbt run
    dbt init
    dbt compile
DBT from tuts

    dbt init
    cd my_project
    dbt debug
    nano ~/.dbt/profiles.yml
    dbt run

in postgres run:

    docker-compose exec dw psql -U dw_user -d dw -c 'SELECT * FROM dw.dev.my_first_dbt_model;'
    docker-compose exec dw psql -U dw_user -d dw -c 'SELECT * FROM dw.dev.my_second_dbt_model;'

in dbt:

    dbt test
    dbt docs generate

[Back to List](#Airflow-DBT-work)
## Aliases
FOR HELP in SHELL TYPE:

    acs (lists all useful shortcuts, docker, python, git, airflow ... etc.)
    acs <keyword> (searches what you need - example: acs git  OR acs docker)
Check for help `aliases = acs <keyword>`
System aliases

    alias c="clear"
    alias r="rm -r"
    alias x="exit"
    alias winhome="/mnt/c/Users/paul/" --- goes to windows desktop
    alias home="cd /home/alexpaul/" --- goes to linux home on main server
    alias home="cd /home/paul/" --- goes to linux home
    
    alias brc="nano ~/.bashrc && source ~/.bashrc"
    alias zrc="nano ~/.zshrc && source ~/.zshrc"
    alias myip="curl ipinfo.io/ip" 
Airflow

    alias reset="cd airflow-pipeline/docker && ./reset.sh && cd"
    alias stop="docker-compose down"
    alias start="docker-compose up -d --build"
    alias restart="stop && start"
    alias rair="_ rm -r airflow-pipeline"
    alias airchmod="_ chmod -R 777 ./logs ./dbt ./docker/reset.sh"
Docker

    alias dc="docker"
    alias dcs="_ service docker start"
    alias dchi="dc run hello-world"
    alias dcair="docker exec -it airflow bash"
Python and pip

    alias pyin="_ apt install python3-pip"
    alias p="pip"
    alias pl="pip list --format=columns"
    alias pin="pip install"
    alias getpip="curl -o get_pip.py https://bootstrap.pypa.io/get-pip.py"
    alias pygetpip="py ./get_pip.py"
    alias invenv="pip install virtualenv"
    alias mkenv="virtualenv"
    alias listpyenv="py -m site"
    
    alias pytest="echo 'def printList(lst):
        for item in lst:
            print(item)
    lst = [1, 2, 3, 4, 5]
    printList(lst)' >> test.py && py test.py && rm test.py"

Gitlab-runner `alias grun="gitlab-runner"`
[Back to List](#Airflow-DBT-work)
***
