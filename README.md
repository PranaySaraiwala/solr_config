# SOLR 8.11.0 INSTALLATION GUIDE

## ASSUMPTIONS:

1. $HOME = /home/ec2-user/
2. Commands are executed at $HOME
3. Zookeeper is running on local machine

### STEPS:

1. Download Solr Tar from Apache Archive .
    wget https://archive.apache.org/dist/lucene/solr/8.11.0/solr-8.11.0.tgz

2. Extract the tar at $HOME
    sudo tar -zxvf solr-8.11.0.tgz .

3. Update Permissions
    chmod -R 777 HOME/solr-8.11.0

4. Create a soft link
    ln -s HOME/solr-8.11.0 solr

5. Start Solr as cloud
    HOME/solr/bin/solr start -e cloud

    Follow Prompts(No of nodes ,Port)

6. Update the "managed-schema" file in the _default configset.
    Path = Home/solr/server/solr/configsets/_default/
    The updated "managed-schema" file is present in the repository.

    Ideally we are adding the below lines in the schema file        

        <dynamicField name="*_S"  type="string"  indexed="true"  stored="true" />
        <dynamicField name="*_D"  type="pdouble"  indexed="true"  stored="true" />
        <dynamicField name="*_DT" type="pdate"  indexed="true"  stored="true" />
        <dynamicField name="*_T"  type="text_general"  indexed="true"  stored="true" />

7. Update the UpdateProcessor Class in 'solrconfig.xml' to make all new created fields as "strings".
    Code Block is shared in the repo.


8. Ensuring that Solr is up. Upload a new config using this updated _default configset schema to your solr instance.

    HOME/solr/bin/solr zk -z localhost:9983 -cmd upconfig -confname <NewConfig Name> -confdir HOME/solr/server/solr/configsets/_default/conf

    Above command assumes Zookeeper is to be locally started at specified port. make zk related changes to the command accordingly.

9. Verify the updated Schema on the Solr UI by using "schema_designer" or by creating a collection out of it and checking its schema.