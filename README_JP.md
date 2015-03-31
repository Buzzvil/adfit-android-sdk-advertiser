# BuzzAd Android SDK For Advertiser 連動ガイド
- 本SDKを通じ、アクション型と起動型広告を進行できます。
- Androidバージョン : 2.2(API Level 8) 以上

## 1. プロジェクトにSDK追加
- [SDK ダウンロード](https://github.com/Buzzvil/buzzad-android-sdk-advertiser/archive/master.zip)後、解凍します。
- buzzad-android-sdk-advertiser.jar ファイルをプロジェクト内（例えば、libsフォルダの中)に追加します。

## 2. AndroidManifest.xml ファイル修正
- AndroidManifest.xml ファイル内に下記のように権限を追加します。
- 該当権限が既にある場合は、次のステップに進んでください。

```
<manifest>
  <application>
    ...
    
  </application>
  ...
  
  <uses-permission android:name="android.permission.INTERNET" />
  
</manifest>
```

## 3. 関数追加
- BuzzAd.init(Context context, String appId) :  アプリ起動時に呼び出します。
- BuzzAd.actionCompleted(Context context) : 起動型はアプリ起動時に、アクション型はアクション完了時に呼び出します。
- 注意1 : 必ずBuzzAd.actionCompleted を呼び出す前に BuzzAd.initを呼び出してください。
- 注意2 : BuzzAd.actionCompleted 呼び出す時にログキャット(タグ:buzzad-sdk)で "api call success" をご確認ください。

### 起動型
- アプリ起動後、最初に呼び出されるアクティビティ作成時点に下記の関数を追加してください。

```
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	
	...
	
	// app_id : 担当者からもらったキーナンバー
	BuzzAd.init(this, "app_id");
	BuzzAd.actionCompleted(this);
}
```

### アクション型(ex. 会員登録, チュートリアル完了など..)
- アプリ起動時、最初に呼び出されるアクティビティ作成時点で BuzzAd.init を呼び出し、
- アクション完了時点で　BuzzAd.actionCompleted を呼び出します。

#### アプリ起動時
```
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	
	...
	
	// app_id : 担当者からもらったキーナンバー
	BuzzAd.init(this, "app_id");
}
```
#### アクション完了時
```
void onAction() {
	
	...
	
	// アクション完了時に呼び出し!
	BuzzAd.actionCompleted(this);
}
```
## 4. テスト後広告出稿
- 上記の過程を通じ連動した apk ファイルを担当者に渡しテスト後、ポイントが反映されたら広告がライブできます。
