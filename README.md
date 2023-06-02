# lab06
После того, как вы настроили взаимодействие с системой непрерывной интеграции, обеспечив автоматическую сборку и тестирование ваших изменений, стоит задуматься о создание пакетов для измениний, которые помечаются тэгами (см. вкладку releases). Пакет должен содержать приложение solver из предыдущего задания

# Task 1

Мы создаем  CMakeLists.txt formatter_ex_libformatter_libsolver_application

CMakeLists.txt Содержание для:formatter_ex_lib
```
cmake_minimum_required(VERSION 3.4)

project(formatter_ex_lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib_dir)

add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)

target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib )

target_link_libraries(formatter_ex_lib formatter_lib)
```
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib_dir)
Эта строка добавляет подкаталог, содержащий formatter_lib, в проект. Второй аргумент formatter_lib_dir задает имя переменной, которая будет содержать путь к этому каталогу.

add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
Это определение библиотеки formatter_ex_lib, которая создается из исходного файла formatter_ex.cpp.

target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib )
Эта строка включает каталог, содержащий formatter_lib, в список каталогов, где нужно искать заголовочные файлы для formatter_ex_lib.

target_link_libraries(formatter_ex_lib formatter_lib)
Эта строка задает список библиотек, с которыми должна быть связана библиотека formatter_ex_lib.


CMakeLists.txt Содержание для:formatter_lib
```
cmake_minimum_required(VERSION 3.4)

project(formatter_lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp ${CMAKE_CURRENT_SOURCE_DIR}/formatter.h)
```


CMakeLists.txt Содержание для:solver_application
```
cmake_minimum_required(VERSION 3.4)

project(solver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(/../formatter_ex_lib formatter_ex_lib_dir)

add_library(solver_lib /../solver_lib/solver.cpp /../solver_lib/solver.h)
add_executable(solver /equation.cpp)

target_include_directories(formatter_ex_lib PUBLIC /../formatter_lib /../formatter_ex_lib /../solver_lib)

target_link_libraries(solver formatter_ex_lib formatter_lib solver_lib)
```
add_subdirectory(/../formatter_ex_lib formatter_ex_lib_dir)
Эта строка добавляет подкаталог formatter_ex_lib в проект. Второй аргумент formatter_ex_lib_dir задает имя переменной, которая будет содержать путь к этому каталогу.


add_library(solver_lib /../solver_lib/solver.cpp /../solver_lib/solver.h)
Эта строка определяет библиотеку solver_lib из исходных файлов solver.cpp и solver.h.


add_executable(solver /equation.cpp)
Эта строка определяет, что исполняемый файл solver будет создан из исходного файла /equation.cpp.


target_include_directories(formatter_ex_lib PUBLIC /../formatter_lib /../formatter_ex_lib /../solver_lib)
Эта строка добавляет каталоги /../formatter_lib, /../formatter_ex_lib и /../solver_lib в список каталогов, где будут искаться заголовочные файлы для библиотеки formatter_ex_lib.

target_link_libraries(solver formatter_ex_lib formatter_lib solver_lib)
Эта строка задает список библиотек, с которыми должен быть связан исполняемый файл solver.
Теперь давайте создадим для связывания все каталогиCMakeLists.txt

CMakeLists.txt содержание:
```
cmake_minimum_required(VERSION 3.4)

project(solver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib formatter_ex_lib_dir)

add_library(solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
add_executable(solver ${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)

target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)

target_link_libraries(solver formatter_ex_lib formatter_lib solver_lib)

install(TARGETS solver
    RUNTIME DESTINATION bin
)

include(CPackConfig.cmake)
```
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib formatter_ex_lib_dir)
Эта строка добавляет подкаталог formatter_ex_lib в проект. Второй аргумент formatter_ex_lib_dir задает имя переменной, которая будет содержать путь к этому каталогу.

add_library(solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
Эта строка определяет библиотеку solver_lib из исходного файла solver.cpp.

add_executable(solver ${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)
Эта строка определяет, что исполняемый файл solver будет создан из исходного файла /solver_application/equation.cpp.

target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)
Эта строка добавляет каталоги ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib, ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib и ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib в список каталогов, где будут искаться заголовочные файлы для библиотеки formatter_ex_lib.

target_link_libraries(solver formatter_ex_lib formatter_lib solver_lib)
Эта строка задает список библиотек, с которыми должен быть связан исполняемый файл solver.

install(TARGETS solver
    RUNTIME DESTINATION bin
)
Эта строка добавляет инструкцию установки для исполняемого файла solver. Она сообщает CMake, что этот файл должен быть установлен в каталоге bin.

include(CPackConfig.cmake)
Эта строка подключает файл CPackConfig.cmake, который используется для настройки установки проекта через CPack.


Коммитим и пушим на гит


Давайте создадим (инструмент упаковки)CPackConfig.cmake

CPackConfig.cmake содержание:
```
include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_CONTACT inkey.cherry@gmail.com)
set(CPACK_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE ${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "C++ app for solving quadratic equations")
set(CPACK_RESOURCE_FILE_LICENSE ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README ${CMAKE_CURRENT_SOURCE_DIR}/README.md)

set(CPACK_SOURCE_IGNORE_FILES  "\\\\.cmake;/build/;/.git/;/.github/")

set(CPACK_SOURCE_INSTALLED_DIRECTORIES "${CMAKE_SOURCE_DIR}; /")

set(CPACK_SOURCE_GENERATOR "TGZ;ZIP")

set(CPACK_DEBIAN_PACKAGE_NAME "solverapp-dev")
set(CPACK_DEBIAN_FILE_NAME "solver-${PRINT_VERSION}.deb")
set(CPACK_DEBIAN_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "all")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "ledibonibell")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)

set(CPACK_GENERATOR "DEB")

include(CPack)
```
1. `include(InstallRequiredSystemLibraries)`: позволяет установить необходимые библиотеки системы.

2. `set(CPACK_PACKAGE_CONTACT inkey.cherry@gmail.com)`: устанавливает контакт для связи по поводу пакета.

3. `set(CPACK_PACKAGE_VERSION ${PRINT_VERSION})`: устанавливает версию пакета.

4. `set(CPACK_PACKAGE_DESCRIPTION_FILE ${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)`: устанавливает файл с описанием пакета.

5. `set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "C++ app for solving quadratic equations")`: устанавливает краткое описание пакета.

6. `set(CPACK_RESOURCE_FILE_LICENSE ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)`: устанавливает лицензионный файл пакета.

7. `set(CPACK_RESOURCE_FILE_README ${CMAKE_CURRENT_SOURCE_DIR}/README.md)`: устанавливает файл README пакета.

8. `set(CPACK_SOURCE_IGNORE_FILES "\\\\.cmake;/build/;/.git/;/.github/")`: задает файлы, которые должны быть проигнорированы при создании исходного пакета.

9. `set(CPACK_SOURCE_INSTALLED_DIRECTORIES "${CMAKE_SOURCE_DIR}; /")`: устанавливает директории, которые должны быть включены в исходный пакет.

10. `set(CPACK_SOURCE_GENERATOR "TGZ;ZIP")`: задает форматы пакетов, которые будут сгенерированы при создании исходного пакета.

11. `set(CPACK_DEBIAN_PACKAGE_NAME "solverapp-dev")`: устанавливает имя пакета в Debian.

12. `set(CPACK_DEBIAN_FILE_NAME "solver-${PRINT_VERSION}.deb")`: задает название файла пакета Debian.

13. `set(CPACK_DEBIAN_PACKAGE_VERSION ${PRINT_VERSION})`: устанавливает версию пакета в Debian.

14. `set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "all")`: устанавливает архитектуру пакета в Debian.

15. `set(CPACK_DEBIAN_PACKAGE_MAINTAINER "ledibonibell")`: устанавливает автора пакета в Debian.

16. `set(CPACK_DEBIAN_PACKAGE_RELEASE 1)`: устанавливает выпуск пакета в Debian.

17. `set(CPACK_GENERATOR "DEB")`: задает формат пакета, который будет сгенерирован.

18. `include(CPack)`: включает инструменты CPack для упаковки приложения в пакеты.
Коммитим и пушим на гит.

Создайте ЛИЦЕНЗИЮ и ОПИСАНИЕ для работы с нашей программой

Создаем два скрипта и упаковываем и запускаем программу на виртуальной машине GitHubAction.ymlRelease.yml

Action.yml содержание:
```
name: CMake

on:
 push:
  branches: [master]
 pull_request:
  branches: [master]

jobs: 
 build_Linux:

  runs-on: ubuntu-latest

  steps:
  - uses: actions/checkout@v3

  - name: Configure Solver
    run: cmake ${{github.workspace}} -B ${{github.workspace}}/build

  - name: Build Solver
    run: cmake --build ${{github.workspace}}/build

```
коммитим и пушим на гит
Release.yml содержание:
```
name: CMake

on:
 push:
   tags:
     - v**

permissions:
  contents: write

jobs: 

  build_packages_Linux:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Configure Solver
      run: cmake ${{github.workspace}} -B ${{github.workspace}}/build -D PRINT_VERSION=${GITHUB_REF_NAME#v}

    - name: Build Solver
      run: cmake --build ${{github.workspace}}/build

    - name: Build package
      run: cmake --build ${{github.workspace}}/build --target package

    - name: Build source package
      run: cmake --build ${{github.workspace}}/build --target package_source

    - name: Make a release
      uses: ncipollo/release-action@v1.10.0
      with:
        artifacts: "build/*.deb,build/*.tar.gz,build/*.zip"
        replacesArtifacts: false
        GITHUB_TOKEN: ${{ secrets.GH_PAT }}
        allowUpdates: true
```
коммитим и пушим на гит
