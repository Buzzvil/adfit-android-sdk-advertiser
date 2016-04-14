# BuzzAd Android SDK For Advertiser - Daily Retention 연동 가이드
- 특정기간 동안 매일 앱을 실행한 유저에게 보상을 지급하는 Daily Retention 광고를 진행하기 위한 버즈애드 광고주 트래킹 라이브러리
- 안드로이드 버전 지원 : Android 2.3(API Level 9) 이상
- 라이브러리 연동을 위해서는 `app_id` 필요(담당자로부터 발급)

## 1. 설정
- [SDK 다운로드](https://github.com/Buzzvil/buzzad-android-sdk-advertiser/archive/master.zip) 후 압축 해제
- 압축 해제한 폴더 내의 buzzad-android-sdk-advertiser.jar 파일을 프로젝트내(예를 들면 libs 폴더 안)에 추가합니다.
- AndroidManifest.xml에 아래와 같이 권한을 추가합니다.(이미 있는 경우 다음 단계로 이동)

```Xml
<manifest>
    ...
    <!-- Permission for BuzzAd-->
    <uses-permission android:name="android.permission.INTERNET" />
</manifest>
```
- **구글 플레이 서비스 라이브러리** 를 설정합니다. [구글 플레이 서비스 라이브러리 설정방법](https://developers.google.com/android/guides/setup) 을 참고하세요.

    > 안드로이드 스튜디오와 이클립스 설정이 다릅니다. 안드로이드 스튜디오인 경우는 **build.gradle > dependencies**에 `compile 'com.google.android.gms:play-services-ads:7.5.0'`만 추가하면 됩니다.

## 2. 트래킹 코드 추가
- `BATracker.init(Context context, String appId)` : 앱 실행 후 처음 호출되는 액티비티의 `onCreate` 에서 호출합니다.
- `BATracker.launch(Context context)` : 실제 버즈애드 서버로 API call 이 이루어지는 함수로, init과 동일하게 앱 실행 후 처음 호출되는 액티비티의 `onCreate` 에서 호출합니다.
- **주의1** : 반드시 `BATracker.launch` 호출하기전에 `BATracker.init`를 호출해야 합니다.
- **주의2** : `BATracker.launch` 호출시에 Logcat(태그:buzzad-analytics)에서 `api call success` 를 확인해야합니다.

### 사용 예시
앱 실행후 처음 호출되는 액티비티 생성 시점에 아래와 같이 두 개의 함수를 추가합니다.

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    
    ...
    
    // app_id : 담당자에게 발급받은 키값
    BATracker.init(this, "app_id");
    BATracker.launch(this);
}
```

## 3. 테스트 완료 후 광고 집행
위 과정을 통해 연동한 apk 파일을 담당자에게 전해주면 테스트 후에 광고가 진행됩니다.