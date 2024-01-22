## Setup
Start everything using docker-compose
```
docker-compose up -d
```

You can now find it open in http://localhost:8888

Login using password: `tensorflow`

#### Load seed data
```
docker cp ./tmp jg-notebook-janusgraph:/opt/tmp/

docker exec -it jg-notebook-janusgraph ./bin/gremlin.sh
```
then in the console:
```
:remote connect tinkerpop.server conf/remote.yaml
g = traversal().withRemote('conf/remote-graph.properties')
path = "/opt/tmp/air-routes.xml";
g.io(path).read().iterate();
```
Quickly testing to get some counts of our new data confirms it worked:
![Screenshot 2024-01-22 125931](/assets/Screenshot%202024-01-22%20125931.png)

Run this at the top of any notebook that you try. Do it here as well.

```
%%graph_notebook_config
{
  "host": "janusgraph",
  "port": 8182,
  "ssl": false,
  "gremlin": {
    "traversal_source": "g"
  }
}
```

Example of how this works is given here: http://localhost:8888/notebooks/notebooks/sample-config.ipynb

#### Run the notebook!
You can find it here: http://localhost:8888/notebooks/notebooks/mySamp.ipynb

## Start a console

Want to just use your standard Gremlin console for whatever reason?

```
docker exec -it jg-notebook-janusgraph ./bin/gremlin.sh
:remote connect tinkerpop.server conf/remote.yaml
g = traversal().withRemote('conf/remote-graph.properties')

# g.V() or whatever you want to run
```
