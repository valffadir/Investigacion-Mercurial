#Gestor de control de versiones Mercurial

##Introducción

Mercurial es una herramienta de gestion y control de versiones distribuida y gratuita. Ofrece la posibilidad de manejar eficientemente proyectos de cualquier dimensión con una intuitiva interfaz. Es fácil de usar y dificil de quebrantar, haciendola una gran alternativa para cualquier persona trabajando con archivos versionados.

##Historia
##Características

  - **Arquitectura distribuida**

Los sistemas gestores de versiones tradicionales (como Subversion) tienen generalmente una arquitectura cliente-servidor, donde un servidor central almacena las revisiones de un proyecto. En contraste, Mercurial es verdaderamente distribuido, permitiéndole a cada desarrollador una copia local del historial de desarrollo completo. De esta forma es posible trabajar independientemente de un servidor central o del acceso a la red. El envío de cambios (committing), ramificaciones (branching) y fusión (merging) de archivos son rápidas y poco costosas.

  - **Rápido**

Las implementaciones y estructuras de datos de Mercurial están diseñadas para ser rápidas. 
Es posible comprobar cambios (diffs) entre revisiones, o regresar a versiones anteriores en segundos. Por ello Mercurial es ideal para grandes proyectos.

  - **Independiencia de la platforma**

Mercurial fue escrito con pensando en la independencia de las plataformas. Por ello, la mayor parte de Mercurial está escrita en Phyton, con una pequeña porción en C por rasones de rendimiento. Como resultado, están disponibles versiones binarias para todas las plataformas mayores.

  - **Extensible**

La funcionalidad de Mercurial se puede incrementar mediante extensiones, ya sea activando las oficiales que se distribuyen junto con Mercurial o descargando algunas de la red o creando las propias. Las esxtensiones están escritas en Phyton y pueden cambiar el funcionamiento de los comandos básicos, agregar nuevos comandos, etc.

  - **Fácil de Usar**

Mercurial presenta un set de comandos con el que la mayoría de los usuarios de sistemas de control de versiones como Subversion se pueden sentir bastante familiarizados. Acciones potencialmente peligrosas se encuentran disponibles al activar extensiones, por lo que la interfaz básica es fácil de usar, fácil de aprender y dificil de quebrantar.

  - **Open Source**

Mercurial es software gratuito licenciado bajo los términos de la GNU General Public License Version 2 o cualquier version más reciente.

##Principios básicos para su úso

Workflow
Prepare Mercurial

As first step, you should teach Mercurial your name. For that you open the file ~/.hgrc (or mercurial.ini in your home directory for Windows) with a text-editor and add the ui section (user interaction) with your username:

[ui]
username = Mr. Johnson <johnson@smith.com>

Initialize the project

Now you add a new folder in which you want to work:

$ hg init project

Add files and track them

Enter the project folder, create some files, then add and commit them.

$ cd project
$ echo 'print("Hello")' > hello.py
$ hg add
$ hg commit
(your default editor opens,
 enter the commit message,
 save and close.)
 
Also you can add only specific files instead of all files in the directory. Mercurial will then track only these files and won't know about the others. The following tells mercurial to track all files whose names begin with "file0" as well as file10, file11 and file12.

$ hg add file0* file10 file11 file12

Save changes

First do some changes:

$ echo 'print("Hello World") > hello.py

see which files changed, which have been added or removed, and which aren't tracked yet

$ hg status

output of hg status:
M hello.py

see the exact changes

$ hg diff

output of hg diff:
diff --git a/hello.py b/hello.py
--- a/hello.py
+++ b/hello.py
@@ -1,1 +1,1 @@
-print("Hello")
+print("Hello World")

commit the changes.

$ hg commit

Copy and move files

When you copy or move files, you should tell Mercurial to do the copy or move for you, so it can track the relationship between the files.

Remember to commit after moving or copying. From the basic commands only commit creates a new revision

$ hg cp hello.py copy
$ hg mv hello.py target
$ hg diff # see the changes
$ hg commit
(enter the commit message)

Check your history

You can look into your initial history with hg log:

$ hg log

This prints a list of changesets along with their date, the user who committed them (you) and their commit message. 

Seeing an earlier revision

Different from the log keeping workflow, you'll want to go back in history at times and do some changes directly there, for example because an earlier change introduced a bug and you want to fix it where it occurred.

To look at a previous version of your code, you can use update. Let's assume that you want to see revision 1.

$ hg update 1

Now your code is back at revision 1, the second commit (Mercurial starts counting at 0). To check if you're really at that revision, you can use identify -n.

$ hg identify -n

To update to the most recent revision, you can use "tip" as revision name.

$ hg update tip

Fixing errors in earlier revisions

When you find a bug in some earlier revision you have two options: either you can fix it in the current code, or you can go back in history and fix the code exactly where you did it, which creates a cleaner history.

To do it the cleaner way, you first update to the old revision, fix the bug and commit it. Afterwards you merge this revision and commit the merge. Don't worry, though: Merging in mercurial is fast and painless, as you'll see in an instant.

Let's assume the bug was introduced in revision 1.

$ hg update 1
$ echo 'print("Hello Mercurial")' > hello.py
$ hg commit

Now the fix is already stored in history. We just need to merge it with the current version of your code.

$ hg merge

If there are conflicts use hg resolve - that's also what merge tells you to do in case of conflicts.

First list the files with conflicts

$ hg resolve --list

Then resolve them one by one. resolve attempts the merge again

$ hg resolve conflicting_file
(fix it by hand, if necessary)

Mark the fixed file as resolved

$ hg resolve --mark conflicting_file

Commit the merge, as soon as you resolved all conflicts. This step is also necessary when there were no conflicts!

$ hg commit

At this point, your fix is merged with all your other work, and you can just go on coding. Additionally the history shows clearly where you fixed the bug, so you'll always be able to check where the bug was.
