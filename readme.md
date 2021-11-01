yarn init
yarn add lerna -D 
lerna init
yarn global add @vue/cli


demo/package.json to configure:
```json
{
    "private": true, // Prohibition of publication

    "publishConfig": {
       "access": "publish" // If the module needs to be published, for the scope module, it needs to be set to publish; otherwise, permission verification is required
    }
}
```

yarn workspaces info

Install / remove dependent modules
Single package workspace

# Package a installing axios
yarn workspace packageA add axios

# Package a remove axios
yarn workspace packageA remove axios


Instalar paquetes para todos los workspace
# root package installation commitzen
yarn add -W -D commitizen

# root package remove commitzen
yarn remove -W commitizen

Run the scripts command for a single package
# Run the dev command of package a
yarn workspace packageA dev
# Here is the run build command on each workspace
yarn workspaces run build

```sh
yarn workspace demo serve
```