# 한자성어 별똥별 Play 스토어 등록 절차

## 앱 버전 정보

- 앱 버전 구분: 앱버전
- 앱 이름: 한자성어 별똥별
- 패키지명: `com.kindrichman.hanjasansungbi`
- 버전 이름: `1.0.0`
- 버전 코드: `1`
- 웹앱 주소: `https://kindrichman-hanja-game.web.app`
- 권장 Android 래핑 방식: Trusted Web Activity(TWA)

## Android 앱 번들 만들기

1. Node.js가 설치된 PC에서 Bubblewrap CLI를 설치합니다.

```powershell
npm install -g @bubblewrap/cli
```

2. 배포된 웹 manifest를 기준으로 TWA 프로젝트를 생성합니다.

```powershell
bubblewrap init --manifest=https://kindrichman-hanja-game.web.app/manifest.webmanifest
```

3. 아래 값으로 설정합니다.

- Package ID: `com.kindrichman.hanjasansungbi`
- App version name: `1.0.0`
- App version code: `1`
- Start URL: `/index.html`
- App name: `한자성어 별똥별`
- Launcher name: `한자별똥별`

4. Android App Bundle을 빌드합니다.

```powershell
bubblewrap build
```

5. Play 스토어에는 APK보다 Android App Bundle 파일인 `.aab`를 업로드합니다.

## Digital Asset Links 설정

TWA가 주소창 없는 앱 화면으로 실행되려면 웹사이트와 Android 앱의 소유권 연결이 필요합니다.

1. Play Console에서 앱을 만든 뒤 `설정 > 앱 무결성 > 앱 서명`으로 이동합니다.
2. `앱 서명 키 인증서`의 SHA-256 인증서 지문을 복사합니다.
3. `play-store/assetlinks.template.json`의 `PLAY_APP_SIGNING_SHA256_CERT_FINGERPRINT`를 실제 SHA-256 값으로 바꿉니다.
4. 파일을 `.well-known/assetlinks.json` 위치에 저장합니다.
5. Firebase Hosting에 다시 배포합니다.

```powershell
firebase.cmd deploy --only hosting
```

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
- 앱 버전 태그는 `app-v1.0.0` 형식으로 남깁니다.
