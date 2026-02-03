# Informe de práctica: Git Large File Storage (LFS)

En este documento detallo el proceso que he seguido para configurar un repositorio con soporte para archivos pesados y gestionar el flujo de trabajo utilizando la extensión **Git Large File Storage** (comúnmente conocido como *Git LFS*).

![Demo](Docs/lfs_07.gif)

## Introducción
Se trata de una extensión de código abierto para el sistema de control de versiones `Git`. Fue diseñada con el objetivo de manejar proyectos que contienen archivos de gran tamaño, muy común en el desarrollo de videojuegos, de manera que está optimizado para reemplazar archivos binarios pesados (como audio, texturas y modelos) por punteros de texto ligero dentro del repositorio.

Algunas de sus características a destacar serían:
- **Punteros ligeros**: En lugar de saturar el historial con *blobs* gigantes, Git guarda una referencia pequeña, almacenando el binario real en un servidor remoto.
- **Descarga ajustada**: Al clonar el repo, solo descarga los archivos binarios necesarios para la versión (`checkout`) actual, no todo el historial histórico de cambios pesados.
- **Gestión vía `.gitattributes`**: Permite definir explícitamente qué extensiones de archivo deben ser tratadas como LFS mediante patrones simples.
- **Transparencia en el flujo**: Se integra con los comandos estándar (`add`, `commit`, `push`), por lo que el usuario trabaja casi igual que con Git normal.

## Configuración inicial y repositorio
Lo primero que hice fue inicializar el repositorio Git en la carpeta del proyecto de la tarea de ejemplo. Una vez abierta la terminal en la ruta del proyecto, ejecuté el comando de instalación `git lfs install`.

![Configuración inicial](Docs/lfs_01.png)

Inicialicé el fichero `.gitignore` con una configuración adecuada para Unity. Esto fue sencillo mediante la búsqueda de best practices y revisando también el fichero compartido en el aula virtual.
````markdown
# Carpetas generadas por Unity y archivos temporales
[Ll]ibrary/
[Tt]emp/
[Oo]bj/
[Bb]uild/
[Bb]uilds/
[Ll]ogs/
[Mm]emoryCaptures/
*.csproj
*.unityproj
*.sln
*.user
*.pidb
*.booproj
*.svd
*.pdb
*.mdb
.vscode/
.DS_Store
````

Configuré el fichero `.gitattributes` para rastrear los tipos de archivos binarios asociados a LFS.
````markdown
# Unity - Archivos de texto (excluir de LFS)
*.cs diff=csharp text
*.shader text

# Unity LFS - Archivos binarios
*.jpg filter=lfs diff=lfs merge=lfs -text
*.png filter=lfs diff=lfs merge=lfs -text
*.psd filter=lfs diff=lfs merge=lfs -text
*.fbx filter=lfs diff=lfs merge=lfs -text
*.mp3 filter=lfs diff=lfs merge=lfs -text
*.wav filter=lfs diff=lfs merge=lfs -text
*.aif filter=lfs diff=lfs merge=lfs -text
*.ttf filter=lfs diff=lfs merge=lfs -text
*.rns filter=lfs diff=lfs merge=lfs -text
````

 Sin realizar aún ningún cambio ejecuté el **commit inicial** para subir la configuración y los archivos base, asegurándome de que el sistema reconocía los atributos antes de seguir.

![Primer commit](Docs/lfs_02.png)


## Creación de la segunda versión (`Nuevos Assets`)
Para cumplir con los requisitos de la práctica, realicé los siguientes pasos en el proyecto:
1.  Importé una **textura** de alta resolución a la carpeta `Assets` con el nombre **`Texturelabs_Concrete_146L`**.
2.  Creé un nuevo script llamado `ScriptTarea1_1`.
3.  Edité el código para que muestre el mensaje *"Script tarea 1.1"* en la consola al iniciarse.
4. Añadí un cubo a la escena y les asigné el script y la textura.

![Escena en Unity](Docs/lfs_03.png)

Una vez terminada esta parte, fui a mi cliente de git (a través de la terminal) y verifiqué el estado con `git status`. Allí revisé que la textura estaba marcada para ser gestionada por LFS y el script como texto plano, y realicé el **commit** con el comentario: *"Agregada textura LFS y script de control"*.

![Segunda versión del proyecto](Docs/lfs_04.png)

## Documentación y entrega (`README`)
Seguí trabajando sobre el repositorio para preparar la entrega final:
* Generé un archivo `README.md` detallando la práctica (el que se está leyendo ahora mismo).
* Grabé y añadí un **Gif animado** de la ejecución mostrando el mensaje en consola.

## Sincronización final (`Push`)
Para la última tarea, procedí a subir todo al repositorio remoto. Ejecuté el comando `push` y observé cómo los objetos LFS se subían también.

![Subida de los ficheros al repo](Docs/lfs_05.png)

Finalmente, verifiqué la subida en la web:
1.  Comprobé que la **textura** tiene la etiqueta *LFS*.
2.  Aseguré que el `README.md` se visualiza correctamente.
3.  Generé el `.zip` del repositorio para la entrega.

![Verificación en repositorio remoto](Docs/lfs_06.png)