## Neo4j Notes

Neo4j is a native graph database lets us build nodes and relationships effortlessly.

### Run Neo4j locally

To run neo4j locally install docker on your os and run following command that will pull docker image and run it,

```bash
docker run -dit --rm --name=my-neo4j -p 7474:7474 -p 7687:7687 --env=NEO4J_AUT
```

to run neo4j REPL run following command that will let us interact with database using `cypher` commands

```bash
docker exec -it my-neo4j cypher-shell
```

**OR**

```bash
winpty docker exec -it my-neo4j cypher-shell
```
