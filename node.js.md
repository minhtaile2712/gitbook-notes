# Node.js

### Install nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

### Check nvm version

```bash
nvm --version
```

### Install Node.js

```bash
nvm install --lts
node --version
```

### Update npm

```bash
npm i -g npm@latest
```

### Uninstall Node.js

```bash
nvm deactivate
nvm uninstall lts/*
```

## On docker



### Node.js on Docker

Best Practices [Reference](https://github.com/nodejs/docker-node/blob/main/docs/BestPractices.md)

#### Environment Variables

```
-e "NODE_ENV=production"
```

#### Handling Kernel Signals

```
docker run -it --init node
```

#### Non-root User

```
-u "node"
```

or in `Dockerfile`:

```
FROM node:6.10.3
...
# At the end, set the user to use when running this image
USER node
```

#### Graceful Shutdown

```js
const gracefulShutdown = () => {
  db.teardown()
    .catch(() => {})
    .then(() => process.exit());
};

process.on("SIGINT", gracefulShutdown);
process.on("SIGTERM", gracefulShutdown);
process.on("SIGUSR2", gracefulShutdown); // Sent by nodemon
```

## Node modules

### fs

```javascript
const location = "/foo/bar";
const dirName = require('path').dirname(location);
if (!fs.existsSync(dirName)) {
    fs.mkdirSync(dirName, { recursive: true });
}
```

### path

```javascript
path.dirname("/foo/bar")
// => /foo
```
