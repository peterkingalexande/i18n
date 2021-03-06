# Mantenimiento de la npm en Node.js

## Paso 1: Clon npm

```console
$ git clone https://github.com/npm/npm.git
$ cd npm
```

si ya has clonado el npm, asegúrese de que el repositorio es actualizado

```console
$ git remote update -p
$ git reset --hard origin latest
```

## Paso 2: Construir el lanzamiento

```console
$ git checkout vX.Y.Z
$ make release
```

Nota: Por favor, corra `npm dist-tag is npm` y, asegúrese de que es la `latest` **dist-tag**. Generalmente, `latest` en el git se lanza en el git como `next` cuando es momento de downstream

## Paso 3: Remover npm anteriores

```console
$ cd /path/to/node
$ git remote update -p
$ git checkout -b npm-x.y.z origin/master
$ cd deps
$ rm -rf npm
```

## Paso 4: Extraer y asentar una nueva npm

```console
$ tar zxf /path/to/npm/release/npm-x.y.z.tgz
$ git add -A npm
$ git commit -m "deps: upgrade npm to x.y.z"
$ cd ..
```

## Paso 5: Actualizar licencias

```console
$ ./configure
$ make -j4
$ ./tools/license-builder.sh
# Los siguentes comandos solo son necesarios si hay cambios
$ git add .
$ git commit -m "doc: update npm LICENSE using license-builder.sh"
```

Nota: Por favor, asegúrese de ser el único haciendo actualizaciones que el npm cambia.

## Paso 6: Aplicar revisión de espacios en blanco

```console
$ git rebase --whitespace=fix master
```

## Paso 7: Análisis de la construcción

```console
$ make test-npm
```