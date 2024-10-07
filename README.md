
# Proxecto Docker Apache

Este proxecto consiste en crear varios servidores Apache usando Docker e compartir o directorio `htdocs` entre eles, mapeando diferentes portos para acceder a cada servidor. A continuación, describo os pasos que seguín para completar o proxecto.

## Pasos realizados

### 1. Comprobar se tiña a imaxe `httpd`

Primeiro, comprobei se xa tiña a imaxe de Apache (`httpd`) descargada. Para iso usei o seguinte comando:

```
docker images
```

Como non a tiña, tiven que descargala coa orde:

```
docker pull httpd
```

### 2. Crear o directorio `htdocs` e solucionar problemas de permisos

Creei un directorio chamado `htdocs` onde gardar a páxina HTML que o servidor Apache vai mostrar. Quixen facer isto no directorio actual do proxecto, pero non tiña permisos para gardar ficheiros nese directorio.

Para solucionar isto, optei por crear un novo directorio dentro do meu directorio persoal, onde sei que teño permisos de escritura:

```
mkdir ~/meu_htdocs
```

Despois disto, puiden traballar sen problemas.

### 3. Crear unha páxina HTML de exemplo

No directorio `~/meu_htdocs`, creei un arquivo chamado `index.html` co seguinte contido:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Páxina de Exemplo</title>
</head>
<body>
    <h1>Benvido ao servidor Apache 2</h1>
    <p>Este é un exemplo de páxina HTML servida dende un contedor Docker.</p>
</body>
</html>
```

### 4. Crear o primeiro contedor `asir_httpd`

Agora que tiña a páxina HTML lista, creei o primeiro contedor chamado `asir_httpd`. Este contedor mapea o porto 8080 da miña máquina ao porto 80 do contedor, e tamén monta o directorio `~/meu_htdocs` no directorio `htdocs` do Apache dentro do contedor.

O comando para crealo foi:

```
docker run -dit --name asir_httpd -p 8080:80 -v ~/meu_htdocs:/usr/local/apache2/htdocs/ httpd
```

### 5. Comprobar a páxina no navegador

Con o contedor en execución, abrín o navegador e fun a:

```
http://localhost:8080
```

Pude ver a páxina HTML que creei, o que indicou que o servidor Apache estaba a funcionar correctamente.

### 6. Crear o segundo contedor `asir_web1`

A continuación, creei un segundo contedor chamado `asir_web1`, mapeando o porto 8000 da miña máquina ao porto 80 do contedor e usando o mesmo directorio `htdocs` que o contedor anterior.

O comando foi:

```
docker run -dit --name asir_web1 -p 8000:80 -v ~/meu_htdocs:/usr/local/apache2/htdocs/ httpd
```

### 7. Crear o terceiro contedor `asir_web2`

Finalmente, creei un terceiro contedor chamado `asir_web2`, esta vez mapeando o porto 8090 da máquina ao porto 80 do contedor, pero usando o mesmo directorio `htdocs` que os outros dous.

O comando foi:

```
docker run -dit --name asir_web2 -p 8090:80 -v ~/meu_htdocs:/usr/local/apache2/htdocs/ httpd
```

### 8. Comprobar que os tres contedores están funcionando

Para asegurarme de que todos os contedores estaban en execución, usei o seguinte comando:

```
docker ps
```

A saída mostrou os tres contedores (`asir_httpd`, `asir_web1` e `asir_web2`) funcionando cos seus respectivos portos mapeados correctamente.

### 9. Comprobar os servidores no navegador

Finalmente, accedín aos seguintes URLs no navegador para comprobar que todos os servidores estaban mostrando a mesma páxina HTML:

- Para `asir_httpd`: [http://localhost:8080](http://localhost:8080)
- Para `asir_web1`: [http://localhost:8000](http://localhost:8000)
- Para `asir_web2`: [http://localhost:8090](http://localhost:8090)

En todos os casos, a mesma páxina HTML apareceu correctamente, o que significa que o directorio `htdocs` está compartido entre todos os contedores.

## Conclusión

Conseguín crear tres contedores de Apache que comparten o mesmo directorio `htdocs`, cada un deles mapeado a un porto diferente da miña máquina. Cada servidor mostra a mesma páxina HTML sen problemas.

O proxecto está listo e pódese ver en funcionamento nos portos 8080, 8000 e 8090.

---

## Comandos utilizados:

- Descargar a imaxe de Apache:
  ```
  docker pull httpd
  ```

- Crear o directorio `htdocs` con permisos suficientes:
  ```
  mkdir ~/meu_htdocs
  ```

- Crear o contedor `asir_httpd`:
  ```
  docker run -dit --name asir_httpd -p 8080:80 -v ~/meu_htdocs:/usr/local/apache2/htdocs/ httpd
  ```

- Crear o contedor `asir_web1`:
  ```
  docker run -dit --name asir_web1 -p 8000:80 -v ~/meu_htdocs:/usr/local/apache2/htdocs/ httpd
  ```

- Crear o contedor `asir_web2`:
  ```
  docker run -dit --name asir_web2 -p 8090:80 -v ~/meu_htdocs:/usr/local/apache2/htdocs/ httpd
  ```

- Comprobar os contedores en execución:
  ```
  docker ps
  ```

