go-ycsb github : https://github.com/pingcap/go-ycsb
# test mysql cluster
./bin/go-ycsb load mysql -P workloads/workloada -p mysql.host=172.21.0.3 -p mysql.port=3306 -p mysql.db=czp -p mysql.user=root -p mysql.password=123456
./bin/go-ycsb run mysql -P workloads/workloada -p mysql.host=172.21.0.3 -p mysql.port=3306 -p mysql.db=czp -p mysql.user=root -p mysql.password=123456

```
***************** properties *****************
"mysql.password"="123456"
"mysql.host"="172.21.0.3"
"readproportion"="0.5"
"dotransactions"="true"
"readallfields"="true"
"mysql.user"="root"
"insertproportion"="0"
"recordcount"="1000"
"scanproportion"="0"
"requestdistribution"="uniform"
"mysql.port"="3306"
"mysql.db"="czp"
"updateproportion"="0.5"
"operationcount"="1000"
"workload"="core"
**********************************************
Run finished, takes 3.954348354s
READ   - Takes(s): 4.0, Count: 523, OPS: 132.3, Avg(us): 582, Min(us): 122, Max(us): 1453, 99th(us): 1000, 99.9th(us): 2000, 99.99th(us): 2000
UPDATE - Takes(s): 3.9, Count: 477, OPS: 120.9, Avg(us): 7601, Min(us): 6324, Max(us): 19349, 99th(us): 14000, 99.9th(us): 20000, 99.99th(us): 20000

```
# test tikv cluster
./bin/go-ycsb load tikv -P workloada -p tikv.pd=127.0.0.1:2379 -p tikv.type=raw
./bin/go-ycsb run tikv -P workloada -p tikv.pd=127.0.0.1:2379 -p tikv.type=raw
```
INFO[0000] [pd] create pd client with endpoints [127.0.0.1:2379]
INFO[0000] [pd] leader switches to: http://127.0.0.1:2379, previous:  
INFO[0000] [pd] init cluster id 6852500187123412278     
***************** properties *****************
"readproportion"="0.5"
"dotransactions"="true"
"recordcount"="1000"
"insertproportion"="0"
"operationcount"="1000"
"readallfields"="true"
"tikv.type"="raw"
"workload"="core"
"requestdistribution"="uniform"
"tikv.pd"="127.0.0.1:2379"
"updateproportion"="0.5"
"scanproportion"="0"
**********************************************
Run finished, takes 5.754616135s
READ   - Takes(s): 5.7, Count: 474, OPS: 82.9, Avg(us): 853, Min(us): 141, Max(us): 38820, 99th(us): 2000, 99.9th(us): 39000, 99.99th(us): 39000
UPDATE - Takes(s): 5.7, Count: 526, OPS: 92.2, Avg(us): 10125, Min(us): 6136, Max(us): 16501, 99th(us): 15000, 99.9th(us): 17000, 99.99th(us): 17000
```
