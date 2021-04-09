## After Staging VM is all set

1. Clone updated monica & nikita repos from git

  *vagrant@stage:~/projects$* `git clone https://github.com/simaiserver/monica.git`
  	
	- Username : simaiserver
	- Password : **As Usual**

  *vagrant@stage:~/projects$* `git clone https://github.com/simaiserver/nikita.git`
  	
	- Username : simaiserver
	- Password : **As Usual**

2. Make New Key Space in Cassandra

  *vagrant@cassandra:~$* `cqlsh 192.168.33.17`
  
  cqlsh> `CREATE KEYSPACE sim_keyspace_stg WITH replication ={'class' :'SimpleStrategy', 'replication_factor' :'1'} AND durable_writes='true';`
  
  cqlsh> `use sim_keyspace_stg;`
  
3. Make New Tables in the Key Space  
  
  cqlsh:use sim_keyspace_stg>  `CREATE TABLE customer_engine_details (customer_spec text, customer_name text, application set<text>, segment text, sub_segment text, engine_model set<text>, certification text, horsepower varint, ebu_price list<tuple<float, float, float>>, dbu_price list<tuple<float, float, float, float, float, float, float, float>>, engine_price list<tuple<float, float, float>>, no_of_engines float, ebu_opts list<tuple<text, text, float>>,  PRIMARY KEY (customer_spec, customer_name));`
  
  cqlsh:use sim_keyspace_stg> `CREATE TABLE customer_and_item (customer_spec text, customer_name text, part_desc text, part_num text, item_type text, sell_price float, quantity int , PRIMARY KEY (customer_spec, customer_name, part_desc));`

4. Install Required Python Modules
  
  *vagrant@stage:~/projects/monica/python_scripts$* `pip install "cassandra_driver==3.22.0"`
  
  *vagrant@stage:~/projects/monica/python_scripts$* `pip install numpy`
  
  *vagrant@stage:~/projects/monica/python_scripts$* `pip install wheel`
  
  *vagrant@stage:~/projects/monica/python_scripts$* `pip install pandas`


5. Get the path 
  
  *vagrant@stage:~/projects/monica/python_scripts$* `pwd`
  
      - OUTPUT : /home/vagrant/projects/monica/python_scripts

6. Change path in all the python scripts

  *vagrant@stage:~/projects/monica/python_scripts$* `vim customer_and_engine_details.py`
  
    - df = pd.read_csv('/home/vagrant/projects/monica/python_scripts/Customer and Engine Details.csv')
    - df_cols_cust_spec = pd.read_csv('/home/vagrant/projects/monica/python_scripts/COLS - Customer Spec.csv')
    - df_dbu_conv_cost = pd.read_csv('/home/vagrant/projects/monica/python_scripts/DBU Conversion Cost.csv')
    - df_one_bms_engine = pd.read_csv('/home/vagrant/projects/monica/python_scripts/OneBMS Engine Sales.csv')
    - session = cluster.connect('sim_keyspace_stg')
    - 
    - One Empty List was not defined in **def prepare_set_one_bms_two(df_data):** function so add **function part_info_list_two = []** 


  *vagrant@stage:~/projects/monica/python_scripts$* `vim customer_and_item_type.py`
  
    - df = pd.read_csv('/home/vagrant/projects/monica/python_scripts/DBU Conversion Cost.csv')
    - session = cluster.connect('sim_keyspace_stg')
    - 
    - REPLACE :  import cassandra.cluster          
    - WITH    :  from cassandra.cluster import Cluster 
    -            from cassandra.auth import PlainTextAuthProvider
    -            
    - REPLACE :  clstr = cassandra.cluster.cluster()
    -            session = clstr.connect('sim_keyspace-stg')
    - WITH    :  auth_provider = PlainTextAuthProvider(username='cassandra', password='cassandra')
    -            cluster = Cluster(['192.168.33.17'],auth_provider = auth_provider, protocol_version=4,)
    -            session = cluster.connect('sim_keyspace_stg')
    -           

7. Run both python scripts

  *vagrant@stage:~/projects/monica/python_scripts$* `python customer_and_engine_details.py`

  *vagrant@stage:~/projects/monica/python_scripts$* `python customer_and_item_type.py`
  
8. Using new key space in cassandra we will se new tables

  *vagrant@cassandra:~$* `cqlsh 192.168.33.17`
  
  cqlsh> `use sim_keyspace_stg;`
  
  cqlsh:use sim_keyspace_stg> `describe tables;`
  
8. Come back to stage VM and install maven
  
  *vagrant@stage:~$* `sudo apt install maven`
  
  *vagrant@stage:~$* `cd /opt/wildfly/standalone/configuration`
  
  *vagrant@stage:/opt/wildfly/standalone/configuration$* `sudo vim standalone.xml`
  
    - <interfaces>    
          <interface name="management">   
              <inet-address value="${jboss.bind.address.management:0.0.0.0}"/>  
      </interface>                 
      <interface name="public">   
              <inet-address value="${jboss.bind.address:0.0.0.0}"/>   
          </interface>                   
      </interfaces>
  
  

  
  
  
  
  
