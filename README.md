## Neo4j Notes

Neo4j is a native graph database that lets us build nodes and relationships effortlessly.

### Terms 

**Cypher:** is a query language in neo4j that lets us query data from a graph.

### Run Neo4j locally

To run neo4j locally install docker on your os and run the following command that will pull the docker image and run it,

```bash
docker run -dit --rm --name=my-neo4j -p 7474:7474 -p 7687:7687 --env=NEO4J_AUT
```

to run neo4j REPL run the following command that will let us interact with the database using `cypher` or commands 

```bash
docker exec -it my-neo4j cypher-shell
```

**OR**

```bash
winpty docker exec -it my-neo4j cypher-shell
```

### Basic Cypher Queries

#### Insert a node:

Syntax for inserting a node

```cypher
CREATE (variable_name: LABEL { ...props })
```

example:

```cypher
CREATE (s1:Site {url: 'https://www.google.com', title: 'Google', description: 'The world\'s most popular search engine.'});
```

### Visualize your data

Neo4j provides a browser experience to see a visual representation of our data. to open it go to [http://localhost:7474/browser/](http://localhost:7474/browser/) and set the authentication type to none since we are just running locally.

to see all the data paste `SELECT (n) RETURN n` inside REPL and run it

![image](https://github.com/user-attachments/assets/71e9b5d9-c1a4-47bf-be33-196b22f7381c)


