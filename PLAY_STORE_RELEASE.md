# 한자성어 별똥별 Play 스토어 등록 절차

## 앱 버전 정보

- 앱 버전 구분: 앱버전
- 앱 이름: 한자성어 별똥별
- 패키지명: `com.kindrichman.hanjasansungbi`
- 버전 이름: `1.0.5`
- 버전 코드: `6`
- 웹앱 주소: `https://kindrichman-hanja-game.web.app`
- Android 래핑 방식: Capacitor Android WebView

## Android 앱 번들 만들기

1. Android Studio를 설치하고 SDK Manager에서 Android SDK를 설치합니다.
2. SDK 경로를 `android/local.properties`에 설정합니다.

```powershell
sdk.dir=C\:\\Users\\USER\\AppData\\Local\\Android\\Sdk
```

3. Node 패키지를 설치하고 Capacitor 웹 자산을 동기화합니다.

```powershell
npm.cmd install
npm.cmd run cap:sync
```

4. Android App Bundle을 빌드합니다.

```powershell
cd android
.\gradlew.bat bundleRelease
```

5. 결과 파일은 보통 아래 경로에 생성됩니다.

```powershell
android\app\build\outputs\bundle\release\app-release.aab
```

6. Play 스토어에는 APK보다 Android App Bundle 파일인 `.aab`를 업로드합니다.

## 설치 테스트

Play Console 업로드 전 휴대폰에서 직접 설치하려면 debug APK를 빌드합니다.

```powershell
cd android
.\gradlew.bat assembleDebug
adb install app\build\outputs\apk\debug\app-debug.apk
```

## Digital Asset Links 설정

현재 앱은 Capacitor WebView 방식이므로 TWA처럼 Digital Asset Links가 필수는 아닙니다.
다만 향후 TWA 방식으로 되돌리거나 웹-앱 소유권 연결이 필요하면 `play-store/assetlinks.template.json`을 사용할 수 있습니다.

## Play Console 등록 순서

1. Google Play Console 개발자 계정을 준비합니다.
2. `앱 만들기`에서 앱 이름, 기본 언어, 앱/게임 여부, 무료/유료 여부를 입력합니다.
3. 앱 액세스, 광고 포함 여부, 콘텐츠 등급, 타겟층, 데이터 보안, 개인정보처리방침 URL을 입력합니다.
4. 스토어 등록정보에 앱 설명, 아이콘, 그래픽 이미지, 스크린샷을 등록합니다.
5. `테스트 및 출시 > 내부 테스트`에서 새 버전을 만들고 `.aab` 파일을 업로드합니다.
6. 내부 테스트로 설치 확인 후, closed/open 테스트 또는 프로덕션으로 승격합니다.

## 업데이트 규칙

- 웹만 바뀌는 경우: Firebase Hosting 배포와 GitHub 커밋만 진행합니다.
- Android 래퍼가 바뀌는 경우: `versionCode`를 반드시 1씩 올립니다.
- Play Console에 새 `.aab`를 올릴 때마다 이전보다 큰 `versionCode`가 필요합니다.
- GitHub 커밋 메시지는 `앱버전: ...` 형식으로 남깁니다.
- 앱 버전 태그는 `app-v1.0.5` 형식으로 남깁니다.
