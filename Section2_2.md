2. 인텐트와 액티비티 실행 살펴보기
=============================
1. 인텐트의 이해
---------------
Intent는 일종의 메시지 객체입니다. 이것을 사용해 다른 앱 구성 요소로부터 작업을 요청할 수 있습니다. 

https://developer.android.com/guide/components/intents-filters.html?hl=ko

2. 인텐트 활용 예시
----------------
######[MainActivity]
```java
Intent intent = new Intent(getApplicationContext(), OtherActivity.class);
intent.putExtra("name" , str);
intent.putExtra("age", 10);
startActivity(intent);
```
######[OtherActivity]
```java
Intent intent = getIntent();
String name = intent.getExtras().getString("name");
int age = intent.getExtras().getInt("age");
```
######cf) 암시적 인텐트
```java
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://m.naver.com"));
startActivity(intent);
```
######cf) startActivityForResult
액티비티를 생성한 후, 생성한 액티비티가 종료되었을 때 기존 액티비티에게 신호를 주기 위한 메서드.
```java
@Override
 protected void onActivityResult(int requestCode, int resultCode, Intent intent) {...}
```
3. 서로 다른 액티비티 실행 방법
---------------------------
```java
Intent intent = new Intent(getApplicationContext(), OtherActivity.class);
startActivity(intent);
```