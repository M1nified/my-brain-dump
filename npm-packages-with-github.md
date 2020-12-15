# Using npm with github packages

```
+ .npmrc
+ Dockerfile
+ package.json
+ github
  + workflow
   + ci.yml
```

```
# .npmrc
@my-org-name:registry=https://npm.pkg.github.com/
//npm.pkg.github.com/:_authToken=${GITHUB_TOKEN_NPM}

```

```json
# package.json
...
"dependencies": {
  "@my-org-name/package-name": "0.0.2",
  ...
}
...
```

```dockerfile
# Dockerfile
FROM node:14.2.0-alpine

ARG GITHUB_TOKEN_NPM # !!!!

WORKDIR /app

COPY package.json .npmrc ./ # !!!!
RUN npm install

COPY ./ ./
RUN rm .npmrc # !!!!
RUN npm run build

EXPOSE 3000

CMD npm run start:prod
```

```yml
# github/workflow/ci.yml
sudo docker build -f ./Dockerfile --build-arg GITHUB_TOKEN_NPM=${{ secrets.NPM_SECRET }} .
```
