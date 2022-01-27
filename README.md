# eficaz
#341 Compatibilidad con Wi-Fi 6e (alfa)

nombre : Android CI
en : [push, pull_request]
trabajos :
  prueba :
    si : " ! contiene (github.event.head_commit.message, 'saltar ci') "
    se ejecuta en : ubuntu-latest
    pasos :
      - usos : acciones/checkout@v1
      - nombre : configurar JDK 16
        usos : acciones/setup-java@v1
        con :
          versión java : 16
      - nombre : pelusa
        ejecutar : bash ./gradlew lintDebug --stacktrace
      - nombre : pruebas unitarias
        ejecutar : bash ./gradlew testDebugUnitTest --stacktrace
      - nombre : cobertura
        ejecutar : bash ./gradlew jacocoTestCoverageVerification --stacktrace
      - nombre : Subir cobertura a Codecov
        usos : codecov/codecov-action@v1
        con :
          ficha : ${{ secretos.CODECOV_TOKEN }}
          archivo : ./app/build/reports/jacoco/jacocoTestReport/jacocoTestReport.xml
