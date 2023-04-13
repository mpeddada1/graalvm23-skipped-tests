# graalvm23-skipped-tests

Reproducer to demonstrate tests missing when junit vintage dependency is not included as a dependency.

1. `mvn clean install -DskipTests`
2. `cd child-module`
3. `mvn test -Pnative`
4. See the following stacktrace:
```build
========================================================================================================================
GraalVM Native Image: Generating 'native-tests' (executable)...
========================================================================================================================
[1/8] Initializing...                                                                                    (5.4s @ 0.16GB)
 Version info: 'GraalVM 23.0.0-dev Java 17.0.7+4-jvmci-23.0-b09 CE'
 Java version info: '17.0.7+4-jvmci-23.0-b09'
 Graal compiler: optimization level: '2', target machine: 'x86-64-v3'
 C compiler: gcc (linux, x86_64, 12.2.0)
 Garbage collector: Serial GC (max heap size: 80% of RAM)
 1 user-specific feature(s)
 - org.graalvm.junit.platform.JUnitPlatformFeature
[junit-platform-native] Running in 'test listener' mode using files matching pattern [junit-platform-unique-ids*] found in folder [${HOME}/graalvm23-skipped-tests/child-module/target/test-ids] and its subfolders.
[2/8] Performing analysis...  [****]                                                                     (9.5s @ 1.19GB)
   4,128 (76.19%) of  5,418 types reachable
   5,048 (53.97%) of  9,354 fields reachable
  18,316 (47.78%) of 38,334 methods reachable
   1,305 types,     1 fields, and   636 methods registered for reflection
      58 types,    58 fields, and    52 methods registered for JNI access
       4 native libraries: dl, pthread, rt, z
[3/8] Building universe...                                                                               (1.6s @ 0.37GB)
[4/8] Parsing methods...      [*]                                                                        (1.1s @ 1.07GB)
[5/8] Inlining methods...     [***]                                                                      (0.5s @ 1.52GB)
[6/8] Compiling methods...    [***]                                                                      (6.4s @ 1.97GB)
[7/8] Layouting methods...    [*]                                                                        (1.8s @ 2.19GB)
[8/8] Creating image...       [**]                                                                       (2.8s @ 2.42GB)
   6.58MB (38.42%) for code area:    10,926 compilation units
   9.78MB (57.10%) for image heap:  135,742 objects and 7 resources
 787.16KB ( 4.49%) for other data
  17.13MB in total
------------------------------------------------------------------------------------------------------------------------
Top 10 origins of code area:                                Top 10 object types in image heap:
   5.02MB java.base                                            1.46MB byte[] for code metadata
 866.73KB svm.jar (Native Image)                               1.22MB java.lang.String
 114.41KB java.logging                                         1.03MB byte[] for general heap data
 112.86KB java.xml                                           966.71KB java.lang.Class
  78.75KB junit-platform-launcher-1.8.1.jar                  918.40KB byte[] for java.lang.String
  76.51KB junit-platform-engine-1.8.1.jar                    517.41KB java.util.HashMap$Node
  62.04KB org.graalvm.nativeimage.base                       354.75KB com.oracle.svm.core.hub.DynamicHubCompanion
  54.94KB jdk.crypto.ec                                      297.53KB byte[] for embedded resources
  35.97KB junit-platform-reporting-1.8.1.jar                 272.20KB java.util.concurrent.ConcurrentHashMap$Node
  29.40KB junit-jupiter-engine-5.8.1.jar                     225.56KB java.util.HashMap$Node[]
  90.85KB for 10 more packages                                 2.16MB for 1074 more object types
------------------------------------------------------------------------------------------------------------------------
Recommendations:
 HEAP: Set max heap for improved and more predictable memory usage.
 CPU:  Enable more CPU features with '-march=native' for improved performance.
------------------------------------------------------------------------------------------------------------------------
                        0.7s (2.4% of total time) in 18 GCs | Peak RSS: 3.54GB | CPU load: 11.68
------------------------------------------------------------------------------------------------------------------------
Produced artifacts:
 ${HOME}/graalvm23-skipped-tests/child-module/target/native-tests (executable)
========================================================================================================================
Finished generating 'native-tests' in 29.8s.
[INFO] Executing: ${HOME}/graalvm23-skipped-tests/child-module/target/native-tests --xml-output-dir ${HOME}/graalvm23-skipped-tests/child-module/target/native-test-reports -Djunit.platform.listeners.uid.tracking.output.dir=${HOME}/graalvm23-skipped-tests/child-module/target/test-ids
JUnit Platform on Native Image - report
----------------------------------------


Test run finished after 3 ms
[         1 containers found      ]
[         0 containers skipped    ]
[         1 containers started    ]
[         0 containers aborted    ]
[         1 containers successful ]
[         0 containers failed     ]
[         0 tests found           ]
[         0 tests skipped         ]
[         0 tests started         ]
[         0 tests aborted         ]
[         0 tests successful      ]
[         0 tests failed          ]

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  32.996 s
[INFO] Finished at: 2023-04-13T20:31:52Z
[INFO] ------------------------------------------------------------------------
```
5. Uncomment these lines in pom.xml and rerun to see:

```build
[1/8] Initializing...                                                                                    (5.6s @ 0.16GB)
 Version info: 'GraalVM 23.0.0-dev Java 17.0.7+4-jvmci-23.0-b09 CE'
 Java version info: '17.0.7+4-jvmci-23.0-b09'
 Graal compiler: optimization level: '2', target machine: 'x86-64-v3'
 C compiler: gcc (linux, x86_64, 12.2.0)
 Garbage collector: Serial GC (max heap size: 80% of RAM)
 1 user-specific feature(s)
 - org.graalvm.junit.platform.JUnitPlatformFeature
[junit-platform-native] Running in 'test listener' mode using files matching pattern [junit-platform-unique-ids*] found in folder [${HOME}/graalvm23-skipped-tests/child-module/target/test-ids] and its subfolders.
[2/8] Performing analysis...  [****]                                                                     (9.7s @ 0.55GB)
   4,537 (77.97%) of  5,819 types reachable
   5,641 (54.40%) of 10,369 fields reachable
  20,720 (50.32%) of 41,175 methods reachable
   1,383 types,    94 fields, and   791 methods registered for reflection
      58 types,    58 fields, and    52 methods registered for JNI access
       4 native libraries: dl, pthread, rt, z
[3/8] Building universe...                                                                               (1.6s @ 1.23GB)
[4/8] Parsing methods...      [*]                                                                        (1.3s @ 0.84GB)
[5/8] Inlining methods...     [***]                                                                      (0.6s @ 1.34GB)
[6/8] Compiling methods...    [***]                                                                      (7.0s @ 1.98GB)
[7/8] Layouting methods...    [*]                                                                        (2.0s @ 2.22GB)
[8/8] Creating image...       [**]                                                                       (3.0s @ 2.47GB)
   7.68MB (39.20%) for code area:    12,476 compilation units
  11.00MB (56.14%) for image heap:  144,801 objects and 7 resources
 935.59KB ( 4.66%) for other data
  19.60MB in total
------------------------------------------------------------------------------------------------------------------------
Top 10 origins of code area:                                Top 10 object types in image heap:
   5.72MB java.base                                            1.67MB byte[] for code metadata
 975.19KB svm.jar (Native Image)                               1.31MB java.lang.String
 113.56KB java.logging                                         1.08MB byte[] for general heap data
 113.30KB java.xml                                             1.07MB java.lang.Class
  85.39KB junit-platform-launcher-1.8.1.jar                  984.25KB byte[] for java.lang.String
  84.79KB junit-platform-engine-1.8.1.jar                    523.17KB java.util.HashMap$Node
  75.96KB junit-4.13.2.jar                                   389.90KB com.oracle.svm.core.hub.DynamicHubCompanion
  62.70KB org.graalvm.nativeimage.base                       297.58KB byte[] for embedded resources
  55.34KB jdk.crypto.ec                                      274.73KB java.util.concurrent.ConcurrentHashMap$Node
  50.77KB jdk.proxy1                                         240.79KB java.lang.String[]
 299.28KB for 15 more packages                                 2.36MB for 1195 more object types
------------------------------------------------------------------------------------------------------------------------
Recommendations:
 HEAP: Set max heap for improved and more predictable memory usage.
 CPU:  Enable more CPU features with '-march=native' for improved performance.
------------------------------------------------------------------------------------------------------------------------
                        0.8s (2.4% of total time) in 21 GCs | Peak RSS: 4.29GB | CPU load: 11.83
------------------------------------------------------------------------------------------------------------------------
Produced artifacts:
 /usr/local/google/home/mpeddada/IdeaProjects/native-image-experiments/graalvm23-skipped-tests/child-module/target/native-tests (executable)
========================================================================================================================
Finished generating 'native-tests' in 31.6s.
[INFO] Executing: ${HOME}/graalvm23-skipped-tests/child-module/target/native-tests --xml-output-dir ${HOME}/graalvm23-skipped-tests/child-module/target/native-test-reports -Djunit.platform.listeners.uid.tracking.output.dir=${HOME}/graalvm23-skipped-tests/child-module/target/test-ids
JUnit Platform on Native Image - report
----------------------------------------

my sample method
com.example.MySampleTest > testSample SUCCESSFUL


Test run finished after 4 ms
[         3 containers found      ]
[         0 containers skipped    ]
[         3 containers started    ]
[         0 containers aborted    ]
[         3 containers successful ]
[         0 containers failed     ]
[         1 tests found           ]
[         0 tests skipped         ]
[         1 tests started         ]
[         0 tests aborted         ]
[         1 tests successful      ]
[         0 tests failed          ]

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  34.777 s
[INFO] Finished at: 2023-04-13T20:36:07Z
[INFO] ------------------------------------------------------------------------

```