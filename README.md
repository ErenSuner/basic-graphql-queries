# GraphQL Game API

A simple GraphQL API for managing video games, authors, and reviews data.

## Overview

This project implements a GraphQL server that provides access to a collection of video games, authors, and game reviews. It allows querying of connected data between these entities as well as mutations to add, update, and delete games.

## Technology Stack

- Node.js
- Apollo Server
- GraphQL

## Project Structure

```
├── index.js              # Server entry point
├── src/
│   ├── _db.js            # In-memory database
│   ├── schema.js         # GraphQL schema definitions
│   └── resolvers.js      # GraphQL resolvers
```

## Getting Started

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn

### Installation

1. Clone the repository
2. Install dependencies:

```bash
npm install
# or
yarn install
```

3. Start the server:

```bash
npm run dev
```

The server will be running at http://localhost:4000

## Data Models

The API provides access to three main types of data:

1. **Games** - Video games with titles and platforms
2. **Authors** - Users who can write reviews
3. **Reviews** - Game reviews with ratings and content

## GraphQL API Usage

### Queries

#### Get All Games

```graphql
query GetAllGames {
  games {
    id
    title
    platform
  }
}
```

#### Get Game Details with Reviews

```graphql
query GetGameWithReviews {
  game(id: "1") {
    title
    platform
    reviews {
      rating
      content
      author {
        name
        verified
      }
    }
  }
}
```

#### Get All Authors

```graphql
query GetAuthors {
  authors {
    id
    name
    verified
  }
}
```

#### Get Author with Reviews

```graphql
query GetAuthorWithReviews {
  author(id: "2") {
    name
    verified
    reviews {
      rating
      content
      game {
        title
        platform
      }
    }
  }
}
```

#### Get All Reviews

```graphql
query GetReviews {
  reviews {
    id
    rating
    content
    author {
      name
    }
    game {
      title
    }
  }
}
```

### Mutations

#### Add a New Game

```graphql
mutation AddGame {
  addGame(game: {
    title: "Cyberpunk 2077"
    platform: ["PC", "PS5", "Xbox"]
  }) {
    id
    title
    platform
  }
}
```

#### Update a Game

```graphql
mutation UpdateGame {
  updateGame(
    id: "3",
    edits: {
      title: "Elden Ring - Deluxe Edition"
      platform: ["PS5", "Xbox", "PC"]
    }
  ) {
    id
    title
    platform
  }
}
```

#### Delete a Game

```graphql
mutation DeleteGame {
  deleteGame(id: "5") {
    id
    title
  }
}
```

## Data Relationships

The schema defines the following relationships:

- A Game has many Reviews
- An Author has many Reviews
- A Review belongs to one Game and one Author

Example of querying these relationships:

```graphql
query RelationshipExample {
  authors {
    name
    reviews {
      rating
      game {
        title
        platform
      }
    }
  }
}
```

## In-Memory Database

This project uses an in-memory database stored in `src/_db.js`. The data is lost when the server restarts. In a production environment, you would want to replace this with a real database.

## Schema Details

The GraphQL schema defines:

- Object types (Game, Review, Author)
- Query type with entry points to fetch data
- Mutation type with operations to modify data
- Input types for mutations

## Contributing

Feel free to submit issues or pull requests to improve the project.

## License

This project is licensed under the MIT License.
