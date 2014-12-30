# BuzzAd Android SDK For Advertiser 연동 가이드
- 본 SDK 를 통해 액션형과 실행형 광고를 진행할 수 있다.
- 안드로이드 버전 요구사항 : 2.2(API Level 8) 이상

## 1. 프로젝트에 SDK 추가
- [SDK 다운로드](https://github.com/Buzzvil/buzzad-android-sdk-advertiser/archive/master.zip) 후 압축을 해제합니다.
- buzzad-android-sdk-advertiser.jar 파일을 프로젝트내(예를 들면 libs 폴더 안)에 추가합니다.

## 2. AndroidManifest.xml 파일 수정
- AndroidManifest.xml 파일 내에 아래와 같이 권한을 추가합니다.
- 해당 권한이 이미 있는경우 다음 단계로 넘어갑니다.

```
<manifest>
  <application>
    ...
    
  </application>
  ...
  
  <uses-permission android:name="android.permission.INTERNET" />
  
</manifest>
```

## 3. 함수 추가
- BuzzAd.init(Context context, String appId) :  앱 실행시 무조건 호출합니다.
- BuzzAd.actionCompleted(Context context) : 실행형은 앱 실행시, 액션형은 액션 완료시에 호출합니다.
- 주의 : 반드시 BuzzAd.actionCompleted 호출하기전에 BuzzAd.init를 호출해야 합니다.

### 실행형
- 앱 실행후 처음 호출되는 액티비티 생성 시점에 아래와 같이 두 개의 함수를 추가합니다.

```
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	
	...
	
	// app_id : 담당자에게 발급받은 키값
	BuzzAd.init(this, "app_id");
	BuzzAd.actionCompleted(this);
}
```

### 액션형(ex. 회원가입, 튜토리얼 완수 등..)
- 앱 실행 시 처음 호출되는 액티비티 생성시점에  BuzzAd.init 를 호출하고,
- 액션완료 시점에 BuzzAd.actionCompleted 를 호출합니다.

#### 앱 실행시
```
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	
	...
	
	// app_id : 담당자에게 발급받은 키값
	BuzzAd.init(this, "app_id");
}
```
#### 액션 완료시
```
void onAction() {
	
	...
	
	// 액션 완료시 호출!
	BuzzAd.actionCompleted(this);
}
```

## 4. 테스트 완료 후 광고 집행
- 위 과정을 통해 연동한 apk 파일을 담당자에게 전해주면 테스트 후에 광고가 진행됩니다.
