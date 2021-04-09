## After Staging VM is all set

1 Always take snapshot of dev and cassandra box before altering anything

  *vagrant@stage:~/projects$* `git clone https://github.com/simaiserver/monica.git`
  	
	- Username : simaiserver
	- Password : **As Usual**

  *vagrant@stage:~/projects$* `git clone https://github.com/simaiserver/nikita.git`
  	
	- Username : simaiserver
	- Password : **As Usual**

2 Making New Key Space in Cassandra

  *vagrant@cassandra:~$* `cqlsh 192.168.33.17`
  
  cqlsh> `CREATE KEYSPACE sim_keyspace_stg WITH replication ={'class' :'SimpleStrategy', 'replication_factor' :'1'} AND durable_writes='true';`
  
  cqlsh> `use sim_keyspace_stg;`
  
  cqlsh:use sim_keyspace_stg>  `CREATE TABLE customer_engine_details (customer_spec text, customer_name text, application set<text>, segment text, sub_segment text, engine_model set<text>, certification text, horsepower varint, ebu_price list<tuple<float, float, float>>, dbu_price list<tuple<float, float, float, float, float, float, float, float>>, engine_price list<tuple<float, float, float>>, no_of_engines float, ebu_opts list<tuple<text, text, float>>,  PRIMARY KEY (customer_spec, customer_name));`
  
  cqlsh:use sim_keyspace_stg> `CREATE TABLE customer_and_item (customer_spec text, customer_name text, part_desc text, part_num text, item_type text, sell_price float, quantity int , PRIMARY KEY (customer_spec, customer_name, part_desc));`
  
  

