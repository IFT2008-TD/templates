# TEMPLATES POUR PROJETS

## INSTRUCTIONS
____________

Vous trouverez dans ce fichier des templates pour les fichiers .gitignore, CMakeLists.txt
et tests/CMakeLists.txt nécessaires pour bâtir les projets du cours et faire vos tests.

Votre projet devrait avoir la structure suivante, ici les noms de fichiers sont fictifs:

```
/RacineDuProjet
   .gitignore
   CMakeLists.txt     
   main.cpp
   classe1.cpp
   classe2.cpp
   classe1.h
   classe2.h
   /tests
        CMakeLists.txt
        mestTests.cpp
        
```

### 1. ```.gitignore```
_____________________

Le fichier ```.gitignore``` sert à avertir git que certains fichiers ou répertoires ne doivent pas être archivés ou suivis.
C'est le cas des répertoires générés par votre IDE.  En général on ne veut garder que les fichiers source et les fichiers
CMakelists.txt


Dans le cas présent, il s'agit des deux répertoires créés par CLion pour stocker les configurations du projet et les 
fichiers pertinents à la construction du projet.

```
.idea/
cmake-build-debug/
```



### 2. ```CMakeLists.txt ```
________________________

C'est le fichier principal du projet.  Il indique à CMake que nous voulons créer un exécutable 
appelé MONPROJET.  Cet exécutable est compilé à partir de la liste de fichiers MESFICHIERSCOMPILABLES.

Il stipule aussi qu'il y a un sous-projet dans le répertoire tests, contenant des tests à être 
exécutés par la librairie googletest.

Vous n'avez qu'à remplacer MONPROJET par le nom d'exécutable voulu, et MESFICHIERSCOMPILABLES par une liste de 
fichiers sources portant l'extension .cpp qui seront compilés dans le projet.

```
cmake_minimum_required(VERSION 3.22)
project([MONPROJET])

set(CMAKE_CXX_STANDARD 11)

add_executable([MONPROJET] main.cpp [MESFICHIERSOMPILABLES])

include(FetchContent)
FetchContent_Declare(
        googletest
        URL https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
)

FetchContent_MakeAvailable(googletest)

enable_testing()

add_subdirectory(tests)

```


### 3. ```tests/CMakeLists.txt```
____________________

Ce fichier indique à CMake comment compiler les tests du projet.  Les tests seront 
dans un exécutable appelé MESTESTS.  Il est compilé à partir d'une liste de 
fichiers sources appelée MESFICHIERSTESTSSOURCE.

Vous n'avez donc qu'à remplacer MESTESTS par le nom de l'exécutable de tests, et 
MESFICHIERSTESTSOURCE par la liste des fichiers nécessaire à la compilation des tests.

```
add_executable(
        [MESTESTS]
        [MESFICHIERSTESTSOURCE]
)

target_include_directories([MESTESTS] PRIVATE ${PROJECT_SOURCE_DIR} )

target_link_libraries(
        [MESTESTS]
        gtest_main
        gtest
        pthread
)

include(GoogleTest)

gtest_discover_tests([MESTESTS])

```

### 4. Exemple

Si on reprend la structure fictive mentionnée au début:
```
/RacineDuProjet
   .gitignore
   CMakeLists.txt     
   main.cpp
   classe1.cpp
   classe2.cpp
   classe1.h
   classe2.h
   /tests
        CMakeLists.txt
        mestTests.cpp
        
```
Ça donnerait les fichiers suivants:

- .gitignore:
```
.idea/
cmake-build-debug/
```

- CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.22)
project(monProjet)

set(CMAKE_CXX_STANDARD 11)

add_executable(monProjet main.cpp classe1.cpp classe2.cpp)

include(FetchContent)
FetchContent_Declare(
        googletest
        URL https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
)

FetchContent_MakeAvailable(googletest)

enable_testing()

add_subdirectory(tests)

```

- tests/CMakeLists.txt:
```
add_executable(
        mesTests
        mesTests.cpp
        classe1.cpp
        classe2.cpp
)

target_include_directories(mesTests PRIVATE ${PROJECT_SOURCE_DIR} )

target_link_libraries(
        mesTests
        gtest_main
        gtest
        pthread
)

include(GoogleTest)

gtest_discover_tests(mesTests)

```