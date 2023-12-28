# My Vue Spotify app

This is a VueJS App which consumes from the Spotify API to display artists searched by the user, albums from these artists and the information about these albums. This project was created for a Fixed technical instance.

## Environment Variables
Client id and client secret values from the Spotify App, which are needed to get the access token for using the API, are stored in a .env file, which is in gitignore, so it's not in this repo. The .env file should be placed in the root of the repository.

## Tech Stack
The project was created with Vite. It uses VueJS 3 with Composition API, Vue Router for the pages and TypeScript.

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Type-Check, Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```
