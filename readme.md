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

Auto version
Automatically determine the version according to the Convention commit specification

There is a feat commit: minor version needs to be updated
fix commit exists: patch version needs to be updated
There is a BREAKING CHANGE submission: a large version needs to be updated

# Generate the changelog file and change the version according to commit
lerna version --conventional-commits

# Generate the changelog file and change the version according to the commit without prompting the user to enter the version
lerna version --conventional-commits --yes
See official documentation lerna version

After successful version, the current branch will be pushed automatically, which can be combined with configuration lerna.json In the file command, the version field configuration allows the branch of version, commit information, etc

{
    "npmClient": "yarn",
    "useWorkspaces": true,
    "version": "0.0.1", 
    "command": {
        "version": {
            "allowBranch": "master",
            "exact": true,
            "ignoreChanges": [
                "**/*.md"
            ],
            "message": "build: release version %v"
        }
    }
}

lerna publish
The function of lerna publish can include version work or simply publish.

See official documentation lerna publish

lerna publish
lerna publish calls lerna version first, and then determines whether to publish to npm

lerna publish from-git
From git is based on the software package submitted by git. Generally, it is submitted through lerna version

lerna publish from-package
From package does not exist in the registry for this version of the latest commit publishing package (I have not used this)

lerna does not publish packages marked private( package.json "private": true)


changelog
According to the Convention commit submission specification, the changelog file can be generated for each package through the tool.

Convention commit support
The use of the Convention commit specification can also be seen in this: Write guide for Commit message and Change log

Install the following dependencies:

```sh
yarn add -W -D commitizen cz-conventional-changelog @commitlint/cli @commitlint/config-conventional husky conventional-changelog-cli
```


commitizen:

A tool for writing qualified commit messages

cz-conventional-changelog:

Commit message format for commitzen to support Angular

to configure package.json Add the following configuration

{
    "config": {
        "commitizen": {
            "path": "./node_modules/cz-conventional-changelog"
        }
    }
}
At this point, you can replace git commit with git cz command to generate a formatted Commit message.

The git cz command has the following options



@commitlint/cli:
commitlint is used to check whether your submission message conforms to the submission format, similar to eslint to verify js syntax

@commitlint/config-conventional:
commitlint verification rule, similar to eslint config standard

Add and configure commitlintrc.js file

module.exports = {
    extends: ['@commitlint/config-conventional'],
    rules: {
        ...otherRules // You can continue to configure your rules
    }
};

husky:
husky is a Git hooks tool, which is used to verify the message content when commit

to configure package.json File, add the following

{
    "husky": {
        "hooks": {
            "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
        }
    }
}

At this point, when a new commit message does not conform to the specification, the commit will be blocked.

conventional-changelog-cli:
Generated according to commit message changelog.md File (here, the function and lerna version -- conventional commits generate changelog partially overlap, which will be differentiated in detail below).

For example: conventional changelog - P angular - I CHANGELOG.md -s -r 2

Note: the changelog generated here is for the entire warehouse, not for each package

Generate changelog
The generation of changelog depends on the above conventional commit specification message

The whole warehouse changelog
Build changes since last release:

conventional-changelog -p angular -i CHANGELOG.md -w
If this is the first time you have used this tool and want to generate all previous change logs, you can do the following:

conventional-changelog -p angular -i CHANGELOG.md -s -r 0
See: conventional-changelog-cli


Each package workspace is a separate changelog
When lerna version is used, the automatic mode -- conventional commitments command generates a changelog for each package workspace at the same time

lerna version --conventional-commits
Note: after lerna version succeeds, a changelog will be generated for each package, including the root package