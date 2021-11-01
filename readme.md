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

Tips: when running the command here, the dependency tree is not detected, but package.json File workspaces configuration workspace to run one by one. lerna build is recommended here. See below lerna build

At this point, it can meet the basic needs without pushing npm.

```sh
yarn workspace demo serve
```

lerna clean
Clear the node used_ Modules directory

lerna clean
lerna diff
The display changes are similar to git diff

lerna diff
lerna ls
List all child package s

lerna ls -l
lerna changed
List the modified sub package s

lerna changed
lerna build
Build all the sub packages, and the sub packages execute build respectively. The - sort parameter can control the command execution according to the topological sorting rules

lerna run --stream --sort build
lerna version
lerna version is used for version bump and supports manual and automatic modes

Manually determine the new version
# Follow the prompts to select the version
lerna version
