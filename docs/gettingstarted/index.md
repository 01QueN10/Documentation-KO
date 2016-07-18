Forge 모드 제작 시작하기
==========================

이 문서는 기본적인 모드를 제작할 수 있도록 안내하며, 여기서 더 나아가기 위한 방향을 제시합니다.

모드 제작 환경 구축하기
--------------------

1. MinecraftForge [파일][files] 사이트에서 배포하는 소스 파일을 받습니다. (Mdk 파일을 받거나, 1.8/1.7 버전에서는 Src 파일을 받으시면 됩니다).
2. 빈 폴더에 소스 파일의 압축을 풀어주세요. 아마 많은 파일들을 볼 수 있을 것인데, 그 중 예제 모드는 `src/main/java` 위치에 있습니다. 오직 일부 파일만이 모드 개발에 꼭 필요할 것이며, 다음 파일들은 당신의 모든 프로젝트에 재사용할 수 있을 것입니다:
    * `build.gradle`
    * `gradlew` (`.bat`및 `.sh`)
    * `gradle` 폴더
3. 이 모드 프로젝트를 위한 폴더를 새로 만드시고, 위의 파일들을 그 폴더로 옮겨주세요.
4. (3)의 폴더에서 명령 프롬프트 창을 열고 `gradlew setupDecompWorkspace`를 실행해 주세요. 이 과정에서 Minecraft와 Forge를 디컴파일(decompile) 및 빌드(build)하는 데 필요한 파일들을 인터넷에서 다운로드하게 되며, Minecraft를 디컴파일하는 과정까지 진행되므로 시간이 좀 걸릴 것입니다. 참고로, 일반적으로 이러한 다운로드 및 디컴파일 과정은 gradle 캐시를 삭제하지 않는 한 한번만 진행하면 됩니다.
5. 모드 제작에 이용할 IDE를 선택하세요: Forge는 명시적으로 Eclipse 및 IntelliJ 환경에서의 개발을 지원하지만, 사실상 Netbeans 부터 vi/emacs까지 어떤 환경에서든 작업이 가능합니다.
    * Eclipse에서는, `gradlew eclipse`를 실행하세요 - 이 과정에서 eclipse 프로젝트에 필요한 파일 몇몇을 다운로드하고, 프로젝트 경로를 설정할 것입니다.
    * IntelliJ에서는, build.gradle 파일을 import하는 것만으로도 충분합니다.
6. 당신의 IDE로 프로젝트를 열어 주세요.
    * Eclipse에서는, 아무데서나 Workspace를 열면 됩니다(모드 프로젝트 폴더의 상위 폴더에서 여시는 것이 가장 쉬울 것입니다). 그 후 모드 프로젝트 폴더를 프로젝트로써 import하면 됩니다.
    * IntelliJ에서는, `gradlew genIntellijRuns`를 실행함으로써 run config를 만들어 주기만 하면 됩니다.

!!! note

    `:decompileMC` task를 실행하는 중 다음과 같은 에러가 발생할 경우 (`setupDecompWorkspace` 중 4번째 단계입니다)

    ```
    Execution failed for task ':decompileMc'.
    GC overhead limit exceeded
    ```

    `org.gradle.jvmargs=-Xmx2G`를 `~/.gradle/gradle.properties` 파일에 (없으면 파일을 생성해서) 추가로 적어넣어 gradle에 더 많은 RAM을 할당해 주세요. 여기서 `~` 는 당신의 유저 [홈 디렉토리(home directory)][] 를 의미합니다.


[home directory]: https://ko.wikipedia.org/wiki/%ED%99%88_%EB%94%94%EB%A0%89%ED%84%B0%EB%A6%AC#%EC%9A%B4%EC%98%81%20%EC%B2%B4%EC%A0%9C%EB%B3%84%20%EA%B8%B0%EB%B3%B8%20%ED%99%88%20%EB%94%94%EB%A0%89%ED%84%B0%EB%A6%AC "운영 체제별 홈 디렉토리"
[files]: http://files.minecraftforge.net "Forge 파일 배포 사이트"

모드 정보 설정하기
--------------------------------

텍스트 파일인 `build.gradle` 파일을 수정하여 모드를 어떻게 빌드(build)할지를 설정할 수 있습니다(파일 이름, 버전 등등을 말이죠).

!!! important

    **절대** build.gradle 파일의 `buildscript {}` 부분을 수정하지 마세요. ForgeGradle이 제대로 작동하기 위해 필수적인 부분입니다.

`apply project: forge`와 `// EDITS GO BELOW HERE`이 적힌 줄 아래로는 거의 모두 바꾸실 수 있으며, 많은 부분들을 제거하거나 마음대로 수정하실 수 있습니다.

Forge에서의 `build.gradle` 파일 작성법만을 위한 사이트가 있습니다 - (영문)[ForgeGradle cookbook][]. 모드 제작 환경 설정이 익숙해지면, 그 사이트에서 유용한 비법(recipe)들을 많이 찾을 수 있을 것입니다.

[forgegradle cookbook]: https://forgegradle.readthedocs.org/en/latest/cookbook/ "The ForgeGradle cookbook"

### 간단한 `build.gradle` 설정법

이 설정들은 어떤 프로젝트에 대해서건 설정하는 것을 추천합니다.

* 결과물 모드 파일의 이름 설정 - `archivesBaseName`의 값을 수정하세요.
* "메이븐 위치(maven coordinates)" 설정 - `group`의 값을 수정하세요.
* 모드의 버전 설정 - `version` 값을 수정하세요.

모드 빌드(build) 및 테스트하기
-----------------------------

1. 모드를 빌드하기 위해서는, 프로젝트 폴더에서 `gradlew build`를 실행하세요. 그러면 `build/libs`에 `[archivesBaseName]-[version].jar`라는 이름의 파일이 생성됩니다. 이 모드 파일을 `mods` 폴더에 넣으면, Forge가 설치된 마인크래프트 환경에서 이 모드를 활성화하게 됩니다. 파일 배포시, 이 모드 jar 파일을 배포하시면 됩니다.
2. 모드를 테스트하려면, 가장 쉬운 방법으로 프로젝트 구성시 자동으로 설정되는 run config를 이용해 실행하는 것이 있으며, 혹은 `gradlew runClient`를 통해 실행하셔도 됩니다. 테스트 실행 시, Minecraft를 `<runDir>` 위치에서 모드 코드가 적용된 상태로 실행하게 됩니다. 이 명령어를 다양한 방면으로 변경할 수 있으며, 이에 대해선 [ForgeGradle cookbook][] 를 참고해 주세요.
3. Dedicated server 환경에서 테스트하기 위해서는, server run config으로 실행하시거나 `gradlew runServer`를 실행해 주시면 됩니다. 그러면 고유 GUI를 가진 마인크래프트 서버를 실행할 것입니다.

!!! note

    언제나, Dedicated server 환경에서 테스트를 하시는 것이 좋습니다. 적어도 서버에서 돌아갈 모드라면 말입니다.
