# SimpleNote app


## How to develop the backend using graphcool 

- https://github.com/graphcool-examples/vue-graphql and remove project.graphcool file.
- `graphcool init`
- `graphcool console`
- update scheme to be like this 

```

# projectId: cj6yo20nm0hxd01342c1qfn9c
# version: 3

type User implements Node {
  id: ID! @isUnique
  createdAt: DateTime!
  updatedAt: DateTime!
  username: String!
  email: String!
  notes: [Note!]! @relation(name: "NoteOnUser")
}

type File implements Node {
  contentType: String!
  createdAt: DateTime!
  id: ID! @isUnique
  name: String!
  secret: String! @isUnique
  size: Int!
  updatedAt: DateTime!
  url: String! @isUnique
}

type Note implements Node {
  title: String!
  context: String
  createdAt: DateTime!
  id: ID! @isUnique
  updatedAt: DateTime!
  user: User @relation(name: "NoteOnUser")
}
```

- `graphcool pull -p cj6yo20nm0hxd01342c1qfn9c`
- `graphcool endpoints` and set endpoints in main.js https://api.graph.cool/simple/v1/cj6yo20nm0hxd01342c1qfn9c to create network interface.


## Using this project
- clone : `$ git clone https://github.com/xmonader/simplenote`

- `cd simplenote`
- `npm install && npm build && npm start`

# Techs used

* [Vue](https://vuejs.org/): Progressive Javascript framework for building user interfaces 
* [Apollo Client](https://github.com/apollographql/apollo-client): Fully-featured, production ready caching GraphQL client
* [Graphcool](https://www.graph.cool): Flexible backend platform combining GraphQL + AWS Lambda
