## Neo4j Notes

Neo4j is a native graph database that lets us build nodes and relationships effortlessly.

### Terms 

**Cypher:** is a query language in neo4j that lets us query data from a graph.

### Run Neo4j locally

To run neo4j locally install docker on your os and run the following command that will pull the docker image and run it,

```bash
docker run -dit --rm --name=my-neo4j -p 7474:7474 -p 7687:7687 --env=NEO4J_AUT
```

flags explanation:

- `-p` 7474:7474 exposes the Neo4j browser interface on port 7474.
- `-p` 7687:7687 exposes the Bolt protocol for Cypher queries on port 7687.
- `--env=NEO4J_AUTH=` sets the default username and password (neo4j/test).

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
CREATE (variable_name: LABEL { ...props });
```

example:

```cypher
CREATE (s1:Site {url: 'https://www.google.com', title: 'Google', description: 'The world\'s most popular search engine.'});
```

example of node insertion with timestamp,

```cypher
CREATE (f:Folder { name: 'Tech', createdAt: datetime() });
```


#### Visualize your data:

Neo4j provides a browser experience that allows us to see a visual representation of our data. to open it go to [http://localhost:7474/browser/](http://localhost:7474/browser/) and set the authentication type to none since we are just running locally.

to see all the data paste `MATCH (n) RETURN n` inside the code cell and run it

![image](https://github.com/user-attachments/assets/7381aa19-924e-44cc-a882-bc6bdd3f51c6)

### Relationships

Syntax to create relationship:

```cypher
CREATE (NODE)-[:LABEL]->(NODE)
```

- `NODE:` Represents a node in the graph.
- `LABEL:` Represents the type of relationship between nodes.

In our case, A folder can contain many sites. To link sites to the folder run the following query,

```cypher
MATCH (f:Folder {name: 'Tech'}), (s:Site)
WHERE s.url IN ['https://www.github.com', 'https://www.stackoverflow.com']
CREATE (f)-[:CONTAINS]->(s)
RETURN f, s;
```

![image](https://github.com/user-attachments/assets/14d1ff67-6b07-4916-8402-1607fddc3261)

#### Nested relationship

To create a nested relationship where a folder is inside another folder, run the following query:

```cypher
MATCH (f:Folder) 
WHERE f.name = 'Tech' 
CREATE (newFolder:Folder { name: 'Web', createdAt: datetime() })
CREATE (f)-[:CONTAINS]->(newFolder)
RETURN f, newFolder;
```

![image](https://github.com/user-attachments/assets/253f709d-2c10-4ea0-8bbe-33b6dd3564df)

To insert a new site in a nested folder run:

```cypher
MATCH (f:Folder) WHERE f.name = 'Web'
CREATE (s:Site {
  url: 'https://developer.mozilla.org/en-US/', 
  title: 'MDN', 
  description: 'The MDN Web Docs site provides information about Open Web technologies including HTML, CSS, and APIs for both Web sites and progressive web apps' 
})
CREATE (f)-[:CONTAINS]->(s)
RETURN f,s;
```

Query to add multiple sites at once:

```cypher
MATCH (f:Folder) WHERE f.name = 'Web'
UNWIND [
  { url: 'https://reactjs.org/', title: 'React', description: 'A JavaScript library for building user interfaces.' },
  { url: 'https://nodejs.org/', title: 'Node.js', description: 'A JavaScript runtime built on Chrome\'s V8 JavaScript engine.' },
  { url: 'https://docs.nestjs.com/', title: 'NestJS', description: 'A progressive Node.js framework for building efficient, reliable, and scalable server-side applications.' }
] AS site
CREATE (s:Site {
  url: site.url, 
  title: site.title, 
  description: site.description
})
CREATE (f)-[:CONTAINS]->(s)
RETURN f, s;
```

![image](https://github.com/user-attachments/assets/d3fe23a6-9119-4370-9cbe-ee5179481ec1)


> In Neo4j's Cypher query language, the `UNWIND` clause is used to transform a list of values into individual rows. Essentially, it "unwraps" a list so that each item in the list becomes a separate row in the result set. This is particularly useful when you want to perform operations on multiple items within a single query.

