# BuzzAd Android SDK For Advertiser 연동 가이드
- 버즈애드를 통해 광고(실행형, 액션형)를 진행하기 위한 버즈애드 광고주 트래킹 라이브러리
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
- 실행형 광고와 액션형 광고는 SDK에서 제공하는 트래킹 코드를 호출해야 하는 시점이 다릅니다.
- `BATracker.init(Context context, String appId)` : 실행형, 액션형 모두 앱 실행 후 처음 호출되는 액티비티의 `onCreate` 에서 호출합니다.
- `BATracker.actionCompleted(Context context)` : 실행형은 앱 실행시, 액션형은 액션 완료시에 호출합니다. 아래의 실행형, 액션형 별 분리된 예시를 참조하세요.
- **주의1** : 반드시 `BATracker.actionCompleted` 호출하기전에 `BATracker.init`를 호출해야 합니다.
- **주의2** : `BATracker.actionCompleted` 호출시에 Logcat(태그:buzzad-analytics)에서 `api call success` 를 확인해야합니다.

### 실행형
앱 실행후 처음 호출되는 액티비티 생성 시점에 아래와 같이 두 개의 함수를 추가합니다.

```Java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	
	...
	
	// app_id : 담당자에게 발급받은 키값
	BATracker.init(this, "app_id");
	BATracker.actionCompleted(this);
}
```

### 액션형(ex. 회원가입, 튜토리얼 완수 등..)
- 앱 실행 시 처음 호출되는 액티비티 생성시점에  `BATracker.init` 를 호출하고,
- 액션완료 시점에 `BATracker.actionCompleted` 를 호출합니다.

##### 앱 실행시
```Java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	
	...
	
	// app_id : 담당자에게 발급받은 키값
	BATracker.init(this, "app_id");
}
```

##### 액션 완료시
```Java
void onAction() {
	
	...
	
	// 액션 완료시 호출!
	BATracker.actionCompleted(this);
}
```

## 3. 테스트 완료 후 광고 집행
위 과정을 통해 연동한 apk 파일을 담당자에게 전해주면 테스트 후에 광고가 진행됩니다.
