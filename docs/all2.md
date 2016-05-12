```
npm docs [<pkgname> [<pkgname> ...]]
npm docs .
npm home [<pkgname> [<pkgname> ...]]
npm home .
```

When installing locally, npm first tries to find an appropriate prefix folder

Starting at the $PWD, npm will walk up the folder tree checking for a folder that contains either a package.json file, or a node_modules folder.

```
foo
+-- blerg@1.2.5
+-- bar@1.2.3
|   +-- blerg@1.x (latest=1.3.7)
|   +-- baz@2.x
|   |   `-- quux@3.x
|   |       `-- bar@1.2.3 (cycle)
|   `-- asdf@*
`-- baz@1.2.3
    `-- quux@3.x
        `-- bar
```

```
foo
+-- node_modules
    +-- blerg (1.2.5) <---[A]
    +-- bar (1.2.3) <---[B]
    |   `-- node_modules
    |       +-- baz (2.0.2) <---[C]
    |       |   `-- node_modules
    |       |       `-- quux (3.2.0)
    |       `-- asdf (2.3.4)
    `-- baz (1.2.3) <---[D]
        `-- node_modules
            `-- quux (3.2.0) <---[E]
```

Even though the latest copy of blerg is 1.3.7, foo has a specific dependency on version 1.2.5. So, that gets installed at [A]. Since the parent installation of blerg satisfies bar's dependency on blerg@1.x, it does not install another copy under [B].

Bar [B] also has dependencies on baz and asdf, so those are installed in bar's node_modules folder. Because it depends on baz@2.x, it cannot re-use the baz@1.2.3 installed in the parent node_modules folder [D], and must install its own copy [C].

Underneath bar, the baz -> quux -> bar dependency creates a cycle. However, because bar is already in quux's ancestry [B], it does not unpack another copy of bar into that folder.

Underneath foo -> baz [D], quux's [E] folder tree is empty, because its dependency on bar is satisfied by the parent folder copy installed at [B].

* npm install <git remote url>:

`<protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>[#<commit-ish>]`

<protocol> is one of git, git+ssh, git+http, git+https, or git+file. If no <commit-ish> is specified, then master is used.