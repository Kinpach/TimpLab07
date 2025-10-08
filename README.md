export GITHUB_USERNAME=Kinpach
export GITHUB_TOKEN=
cd ${GITHUB_USERNAME}/workspace
pushd .
source scripts/activate
git clone https://github.com/${GITHUB_USERNAME}/TimpLab03 projects/TimpLab06
Клонирование в «projects/TimpLab06»...
remote: Enumerating objects: 56, done.
remote: Counting objects: 100% (56/56), done.
remote: Compressing objects: 100% (29/29), done.
remote: Total 56 (delta 19), reused 52 (delta 18), pack-reused 0 (from 0)
Получение объектов: 100% (56/56), 15.59 КиБ | 149.00 КиБ/с, готово.
Определение изменений: 100% (19/19), готово.
cd projects/TimpLab06
git remote remove origin
git remote add origin https://github.com/${GITHUB_USERNAME}/TimpLab06
cat > .travis.yml <<EOF
language: cpp
EOF
cat >> .travis.yml <<EOF
script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
cat >> .travis.yml <<EOF
addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
git add .travis.yml
git add README.md
git commit -m"added CI"
[master 3230758] added CI
 2 files changed, 13 insertions(+)
 create mode 100644 .travis.yml
git push origin master
Username for 'https://github.com': Kinpach
Password for 'https://Kinpach@github.com': 
Перечисление объектов: 60, готово.
Подсчет объектов: 100% (60/60), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (32/32), готово.
Запись объектов: 100% (60/60), 16.04 КиБ | 8.02 МиБ/с, готово.
Всего 60 (изменений 21), повторно использовано 54 (изменений 19), повторно использовано пакетов 0
remote: Resolving deltas: 100% (21/21), done.
To https://github.com/Kinpach/TimpLab06
 * [new branch]      master -> master

ls
 BMSTU           tasks          Документы     непонятно
 Kinpach        'visual code'   Загрузки      Общедоступные
 reports         бандит         Изображения  'Рабочий стол'
 snap            Видео          Музыка        Шаблоны
cd Kinpach
ls
workspace
cd workspace
ls
LICENSE  node  projects  README.md  reports  scripts  tasks
cd projects
ls
AL-Labs-IY8-25  lab_02  TimpLab02  TimpLab03  TimpLab04  TimpLab06
rm -rf TimpLab06
ls
AL-Labs-IY8-25  lab_02  TimpLab02  TimpLab03  TimpLab04
export GITHUB_USERNAME=Kinpach
alias gsed=sed
cd
export GITHUB_USERNAME=Kinpach
alias gsed=sed
cd ${GITHUB_USERNAME}/workspace
pushd .
source scripts/activate
git clone https://github.com/${GITHUB_USERNAME}/TimpLab04 projects/TimpLab06
Клонирование в «projects/TimpLab06»...
remote: Enumerating objects: 63, done.
remote: Counting objects: 100% (63/63), done.
remote: Compressing objects: 100% (33/33), done.
remote: Total 63 (delta 23), reused 59 (delta 21), pack-reused 0 (from 0)
Получение объектов: 100% (63/63), 17.42 КиБ | 5.81 МиБ/с, готово.
Определение изменений: 100% (23/23), готово.
cd projects/TimpLab06
git remote remove origin
git remote add origin https://github.com/${GITHUB_USERNAME}/TimpLab06
mkdir third-party
git submodule add https://github.com/google/googletest third-party/gtest
Клонирование в «/home/Kinpach/workspace/projects/TimpLab06/third-party/gtest»...
remote: Enumerating objects: 28036, done.
remote: Counting objects: 100% (257/257), done.
remote: Compressing objects: 100% (166/166), done.
remote: Total 28036 (delta 164), reused 92 (delta 91), pack-reused 27779 (from 4)
Получение объектов: 100% (28036/28036), 13.53 МиБ | 368.00 КиБ/с, готово.
Определение изменений: 100% (20765/20765), готово.
cd third-party/gtest && git checkout release-1.11.0 && cd ../..
Примечание: переключение на «release-1.11.0».

Вы сейчас в состоянии «отсоединённого указателя HEAD». Можете осмотреться,
внести экспериментальные изменения и зафиксировать их, также можете
отменить любые коммиты, созданные в этом состоянии, не затрагивая другие
ветки, переключившись обратно на любую ветку.

Если хотите создать новую ветку для сохранения созданных коммитов, можете
сделать это (сейчас или позже), используя команду switch с параметром -c.
Например:

  git switch -c <новая-ветка>

Или отмените эту операцию с помощью:

  git switch -

Отключите этот совет, установив переменную конфигурации
advice.detachedHead в значение false

HEAD сейчас на e2239ee6 Googletest export
git add third-party/gtest
git commit -m"added gtest framework"
[master 6c01ef0] added gtest framework
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest
gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
cat >> CMakeLists.txt <<EOF

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
EOF
mkdir tests
cat > tests/test1.cpp <<EOF
#include <print.hpp>

#include <gtest/gtest.h>

TEST(Print, InFileStream)
{
  std::string filepath = "file.txt";
  std::string text = "hello";
  std::ofstream out{filepath};

  print(text, out);
  out.close();

  std::string result;
  std::ifstream in{filepath};
  in >> result;

  EXPECT_EQ(result, text);
}
EOF
cmake -H. -B_build -DBUILD_TESTS=ON
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- The C compiler identification is GNU 13.3.0
-- The CXX compiler identification is GNU 13.3.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Deprecation Warning at third-party/gtest/CMakeLists.txt:4 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at third-party/gtest/googlemock/CMakeLists.txt:45 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at third-party/gtest/googletest/CMakeLists.txt:56 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Found Python: /usr/bin/python3 (found version "3.12.3") found components: Interpreter 
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE  
-- Configuring done (1.1s)
-- Generating done (0.0s)
-- Build files have been written to: /home/Kinpach/workspace/projects/TimpLab06/_build
cmake --build _build
[  8%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 16%] Linking CXX static library libprint.a
[ 16%] Built target print
[ 25%] Building CXX object third-party/gtest/googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 33%] Linking CXX static library ../../../lib/libgtest.a
[ 33%] Built target gtest
[ 41%] Building CXX object third-party/gtest/googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Linking CXX static library ../../../lib/libgtest_main.a
[ 50%] Built target gtest_main
[ 58%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[ 66%] Linking CXX executable check
[ 66%] Built target check
[ 75%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 83%] Linking CXX static library ../../../lib/libgmock.a
[ 83%] Built target gmock
[ 91%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../../../lib/libgmock_main.a
[100%] Built target gmock_main
cmake --build _build --target test
Running tests...
Test project /home/Kinpach/workspace/projects/TimpLab06/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.01 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
_build/check
Running main() from /home/Kinpach/workspace/projects/TimpLab06/third-party/gtest/googletest/src/gtest_main.cc
[==========] Running 1 test from 1 test suite.
[----------] Global test environment set-up.
[----------] 1 test from Print
[ RUN      ] Print.InFileStream
[       OK ] Print.InFileStream (0 ms)
[----------] 1 test from Print (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test suite ran. (0 ms total)
[  PASSED  ] 1 test.
cmake --build _build --target test -- ARGS=--verbose
Running tests...
UpdateCTestConfiguration  from :/home/Kinpach/workspace/projects/TimpLab06/_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/home/Kinpach/workspace/projects/TimpLab06/_build/DartConfiguration.tcl
Test project /home/Kinpach/workspace/projects/TimpLab06/_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: check

1: Test command: /home/Kinpach/workspace/projects/TimpLab06/_build/check
1: Working Directory: /home/Kinpach/workspace/projects/TimpLab06/_build
1: Test timeout computed to be: 10000000
1: Running main() from /home/Kinpach/workspace/projects/TimpLab06/third-party/gtest/googletest/src/gtest_main.cc
1: [==========] Running 1 test from 1 test suite.
1: [----------] Global test environment set-up.
1: [----------] 1 test from Print
1: [ RUN      ] Print.InFileStream
1: [       OK ] Print.InFileStream (0 ms)
1: [----------] 1 test from Print (0 ms total)
1: 
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test suite ran. (0 ms total)
1: [  PASSED  ] 1 test.
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
gsed -i 's/TimpLab04/TimpLab06/g' README.md
gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml
gsed -i '/cmake --build _build --target install/a\
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
travis lint 
Команда «travis» не найдена, но может быть установлена с помощью:
sudo snap install travis  # version 1.8.9, or
sudo apt  install travis  # version 220729-1
См. 'snap info travis', чтобы посмотреть дополнительные версии.
git add .travis.yml
git add tests
git add -p
diff --git a/CMakeLists.txt b/CMakeLists.txt
index 96a361e..aa7a323 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -4,6 +4,7 @@ set(CMAKE_CXX_STANDARD 11)
 set(CMAKE_CXX_STANDARD_REQUIRED ON)
 
 option(BUILD_EXAMPLES "Build examples" OFF)
+option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
 
(1/2) Индексировать этот блок [y,n,q,a,d,j,J,g,/,e,?]? y
@@ -34,3 +35,12 @@ install(TARGETS print
 
 install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
 install(EXPORT print-config DESTINATION cmake)
+
+if(BUILD_TESTS)
+  enable_testing()
+  add_subdirectory(third-party/gtest)
+  file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
+  add_executable(check ${${PROJECT_NAME}_TEST_SOURCES})
+  target_link_libraries(check ${PROJECT_NAME} gtest_main)
+  add_test(NAME check COMMAND check)
+endif()
(2/2) Индексировать этот блок [y,n,q,a,d,K,g,/,e,?]? n

diff --git a/README.md b/README.md
index 3af970a..a5c5d2d 100644
--- a/README.md
+++ b/README.md
@@ -4,27 +4,27 @@ export GITHUB_USERNAME=Kinpach
 cd ${GITHUB_USERNAME}/workspace
 pushd .
 source scripts/activate
-git clone https://github.com/${GITHUB_USERNAME}/TimpLab03 projects/TimpLab04
-Клонирование в «projects/TimpLab04»...
+git clone https://github.com/${GITHUB_USERNAME}/TimpLab03 projects/TimpLab06
+Клонирование в «projects/TimpLab06»...
 remote: Enumerating objects: 56, done.
 remote: Counting objects: 100% (56/56), done.
 remote: Compressing objects: 100% (29/29), done.
 remote: Total 56 (delta 19), reused 52 (delta 18), pack-reused 0 (from 0)
 Получение объектов: 100% (56/56), 15.59 КиБ | 149.00 КиБ/с, готово.
 Определение изменений: 100% (19/19), готово.
-cd projects/TimpLab04
-git remote remove origin
-git remote add origin https://github.com/${GITHUB_USERNAME}/TimpLab04
-cat > .travis.yml <<EOF
+cd projects/TimpLab06
+git remote remove origin
+git remote add origin https://github.com/${GITHUB_USERNAME}/TimpLab06
+cat > .travis.yml <<EOF
 language: cpp
EOF
-cat >> .travis.yml <<EOF
+cat >> .travis.yml <<EOF
 script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
-cat >> .travis.yml <<EOF
+cat >> .travis.yml <<EOF
 addons:
  apt:
    sources:
(1/3) Индексировать этот блок [y,n,q,a,d,j,J,g,/,s,e,?]? a

git commit -m"added tests"
[master 593e103] added tests
 4 files changed, 36 insertions(+), 15 deletions(-)
 create mode 100644 tests/test1.cpp
git push origin master
Username for 'https://github.com': Kinpach
Password for 'https://Kinpach@github.com': 
Перечисление объектов: 74, готово.
Подсчет объектов: 100% (74/74), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (40/40), готово.
Запись объектов: 100% (74/74), 18.94 КиБ | 6.31 МиБ/с, готово.
Всего 74 (изменений 28), повторно использовано 60 (изменений 23), повторно использовано пакетов 0
remote: Resolving deltas: 100% (28/28), done.
To https://github.com/Kinpach/TimpLab06
 * [new branch]      master -> master
mkdir artifacts
sleep 20s && gnome-screenshot --file artifacts/screenshot.png


export GITHUB_USERNAME=Kinpach
export GITHUB_EMAIL=litclick@yandex.ru
alias edit=nano
alias gsed=sed
cd ${GITHUB_USERNAME}/workspace
pushd . 
source scripts/activate
git clone https://github.com/${GITHUB_USERNAME}/TimpLab05 projects/TimpLab06
Клонирование в «projects/TimpLab06»...
remote: Enumerating objects: 77, done.
remote: Counting objects: 100% (77/77), done.
remote: Compressing objects: 100% (38/38), done.
remote: Total 77 (delta 30), reused 72 (delta 28), pack-reused 0 (from 0)
Получение объектов: 100% (77/77), 25.21 КиБ | 3.15 МиБ/с, готово.
Определение изменений: 100% (30/30), готово.
cd projects/TimpLab06
git remote remove origin
git remote add origin https://github.com/${GITHUB_USERNAME}/TimpLab06
gsed -i '/project(print)/a\
set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")
' CMakeLists.txt
gsed -i '/project(print)/a\
set(PRINT_VERSION\
  \${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
gsed -i '/project(print)/a\
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
gsed -i '/project(print)/a\
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
gsed -i '/project(print)/a\
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
gsed -i '/project(print)/a\
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
git diff
diff --git a/CMakeLists.txt b/CMakeLists.txt
index 602e661..c94edcc 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -7,6 +7,13 @@ option(BUILD_EXAMPLES "Build examples" OFF)
 option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
+set(PRINT_VERSION_MAJOR 0)
+set(PRINT_VERSION_MINOR 1)
+set(PRINT_VERSION_PATCH 0)
+set(PRINT_VERSION_TWEAK 0)
+set(PRINT_VERSION
+  ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
+set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
 
 add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
 
touch DESCRIPTION && edit DESCRIPTION
touch ChangeLog.md
export DATE="`LANG=en_US date +'%a %b %d %Y'`"
cat > ChangeLog.md <<EOF
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
cat > CPackConfig.cmake <<EOF
include(InstallRequiredSystemLibraries)
EOF
cat >> CPackConfig.cmake <<EOF
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")
EOF
cat >> CPackConfig.cmake <<EOF
set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
cat >> CPackConfig.cmake <<EOF
set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
cat >> CPackConfig.cmake <<EOF
set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
cat >> CPackConfig.cmake <<EOF
include(CPack)
EOF
cat >> CMakeLists.txt <<EOF
include(CPackConfig.cmake)
EOF
gsed -i 's/TimpLab05/TimpLab06/g' README.md
git add .
git commit -m"added cpack config"
[master cfb102b] added cpack config
 5 files changed, 112 insertions(+), 82 deletions(-)
 create mode 100644 CPackConfig.cmake
 create mode 100644 ChangeLog.md
 create mode 100644 DESCRIPTION
git tag v0.1.0.0
git push origin master --tags
Username for 'https://github.com': Kinpach
Password for 'https://Kinpach@github.com': 
Перечисление объектов: 83, готово.
Подсчет объектов: 100% (83/83), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (42/42), готово.
Запись объектов: 100% (83/83), 26.61 КиБ | 6.65 МиБ/с, готово.
Всего 83 (изменений 33), повторно использовано 74 (изменений 30), повторно использовано пакетов 0
remote: Resolving deltas: 100% (33/33), done.
To https://github.com/Kinpach/TimpLab06
 * [new branch]      master -> master
 * [new tag]         v0.1.0.0 -> v0.1.0.0
cmake -H. -B_build
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- The C compiler identification is GNU 13.3.0
-- The CXX compiler identification is GNU 13.3.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.7s)
-- Generating done (0.0s)
-- Build files have been written to: /home/Kinpach/workspace/projects/TimpLab06/_build
cmake --build _build
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
cd _build
cpack -G "TGZ"
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/Kinpach/workspace/projects/TimpLab06/_build/print-0.1.0.0-Linux.tar.gz generated.
cd ..
cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/Kinpach/workspace/projects/TimpLab06/_build
cmake --build _build --target package
[100%] Built target print
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/Kinpach/workspace/projects/TimpLab06/_build/print-0.1.0.0-Linux.tar.gz generated.
mkdir artifacts
mv _build/*.tar.gz artifacts
tree artifacts
artifacts
└── print-0.1.0.0-Linux.tar.gz

1 directory, 1 file
