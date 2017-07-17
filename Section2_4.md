4. 네비게이션 드로어 및 구글 맵
===========================
1.네비게이션 드로어(Navigation Drawer)
----------------------------------- 
네비게이션 드로어는 왼쪽 가장자리에 있습니다. 대부분의 경우 숨겨져 있지만 사용자가 손가락으로 화면의 왼쪽 가장자리를 스와이프하거나 앱의 최상위에서 사용자가 작업 모음의 앱 아이콘을 터치하면 표시됩니다.

[구현]
1. Gradle : compile 'com.android.support:design:25.0.0' 추가
   style.xml : Theme.AppCompat.Light.NoActionBar 수정

2. 툴바를 xml 및 자바 코드에서 세팅한다.
[activity_main.xml]
```xml
<android.support.v7.widget.Toolbar
android:layout_width="match_parent"
android:layout_height="?attr/actionBarSize"
android:theme="@style/ThemeOverlay.AppCompat.Dark"
android:background="?attr/colorPrimary"
android:id="@+id/toolbar"
>
</android.support.v7.widget.Toolbar>
```
[MainActivity.java]
```java
Toolbartoolbar=(Toolbar)findViewById(R.id.toolbar);
setSupportActionBar(toolbar);
ActionBaractionBar=getSupportActionBar();
actionBar.setHomeAsUpIndicator(R.drawable.ic_menu);
actionBar.setDisplayHomeAsUpEnabled(true);
```
3. NavigationView 추가 
	- DrawerLayout을 부모로!
	- android:fitsSystemWindows="true" <!--뷰가 차지할 수 있는 영역 확장-->
	- 네비게이션 뷰의 헤더와 메뉴를 추가로 구현해 주어야 한다.
cf) 2번째 과제할 때 툴바를 AppBarLayout으로 감싸서 구현했다.
```xml	
<android.support.v4.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true">
 
    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">
 
        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            android:theme="@style/ThemeOverlay.AppCompat.Dark" />
 
    </RelativeLayout>
 
    <android.support.design.widget.NavigationView
        android:id="@+id/navigation_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:headerLayout="@layout/drawer_header"
        app:menu="@menu/drawer"/>
</android.support.v4.widget.DrawerLayout>
```
4. 메뉴 구현하기 : /res/menu 생성 후 리소스 파일을 생성한다.
추가할 아이템을 여기에 써주면 된다.
```xml
<?xml version="1.0"encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
<group android:checkableBehavior="single">
<item
android:id="@+id/tab_destination"
android:checked="true"
android:icon="@drawable/ic_attachment"
android:title="거리순"/>
```
5. 네비게이션의 헤더 레이아웃 만들기 : 헤더에 표시될 레이아웃을 만들자.

```xml
<?xml version="1.0"encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="150dp"
android:background="?attr/colorPrimaryDark"
android:padding="16dp"
android:theme="@style/ThemeOverlay.AppCompat.Dark"
android:gravity="bottom"
>

<TextView
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:text="맛집리스트"
android:textAppearance="@style/TextAppearance.AppCompat.Body1"/>
</LinearLayout>
```
6. 메인액티비티 구현(아이템이 선택됐을 때 리스너,햄버거 클릭됐을 때 드로어 열어주기)

```java
navigationView.setNavigationItemSelectedListener(new NavigationView.OnNavigationItemSelectedListener(){
@Override
public boolean onNavigationItemSelected(MenuItemmenuItem){
menuItem.setChecked(true);
mDrawerLayout.closeDrawers();

int id=menuItem.getItemId();
switch(id){
caseR.id.tab_destination:
Toast.makeText(MainActivity.this,menuItem.getTitle(),Toast.LENGTH_LONG).show();
break;
//to implement
}
return true;
}
});

}

@Override
public boolean onOptionsItemSelected(MenuItem item){
int id=item.getItemId();

switch(id){
caseandroid.R.id.home:
mDrawerLayout.openDrawer(GravityCompat.START);
return true;
}

return super.onOptionsItemSelected(item);
}
```

2.하단 네비게이션
--------------
1. Gradle : compile 'com.android.support:design:25.0.0' 추가
2. menu.xml에 아이템 정의하기(3개 이상! 해주어야한다)
```xml
<?xml version="1.0"encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto">
<item
android:id="@+id/action_one"
app:showAsAction="ifRoom"
android:enabled="true"
android:icon="@android:drawable/ic_dialog_info"
android:title="HOME"/>
<item
android:id="@+id/action_two"
app:showAsAction="ifRoom"
android:icon="@android:drawable/ic_dialog_info"
android:title="LIST"/>
<item
android:id="@+id/action_three"
app:showAsAction="ifRoom"
android:enabled="true"
android:icon="@android:drawable/ic_dialog_info"
android:title="ETC"/>
</menu>
```
3.BottomNavigationView 추가
```xml
<android.support.design.widget.BottomNavigationView
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:id="@+id/bottomNavi"
android:layout_alignParentBottom="true"
app:itemBackground="@color/colorPrimary"
app:itemIconTint="@color/colorAccent"
app:itemTextColor="#ffffff"
app:layout_behavior="tech.thdev.app.view.BottomNavigationBehavior"
app:menu="@menu/menu_item"
>
<!--app:behavior스크롤이발생할경우바텀메뉴는숨김처리하며다시위로올라오면노출할것-->
</android.support.design.widget.BottomNavigationView>
```
4.메인액티비티에서 아이템 클릭했을 때 리스너 달아주기
```java
bottomNavi.setOnNavigationItemSelectedListener(new BottomNavigationView.OnNavigationItemSelectedListener(){
@Override
public boolean onNavigationItemSelected(@NonNullMenuItemitem){
switch(item.getItemId()){
caseR.id.item_home:
return true;
caseR.id.item_list:
return true;
caseR.id.item_etc:
return true;
}
return false;
}
});
```

3.구글 맵 프로젝트
----------------
#####[기본 구현 : 위도 경도에 따른 위치 + 마커]

1.세팅
	• gradle에 추가
``
compile'com.google.android.gms:play-services-maps:11.0.2'
    compile'com.google.android.gms:play-services-location:11.0.2'
``
	• https://console.developers.google.com/ 에서 사용자 인증정보 
	• 패키지 이름, SHA-1을 입력하고 API키 생성
	• manifest에 API키 입력하고 퍼미션 주기

```
<meta-data
android:name="com.google.android.geo.API_KEY"
android:value="AIza..."/>

<uses-permisson
android:name="android.permission.ACCESS_FINE_LOCATION">
```
2. 액티비티의 레이아웃에 지도 위치 만들어주기.
```xml
<fragment
android:layout_width="match_parent"
android:layout_height="match_parent"
android:id="@+id/map"
class="com.google.android.gms.maps.MapFragment"></fragment>
<!--MapFragment는앱에지도를표시하기위해사용되는컴포넌트로관련자동처리-->
```
3. 메인 액티비티에 OnMapReadyCallback을 implement해주고…

```java
@Override
protected void onCreate(BundlesavedInstanceState){
//...
FragmentManager fragmentManager=getFragmentManager();
MapFragment mapFragment= (MapFragment)fragmentManager.findFragmentById(R.id.map);
//프래그먼트에 리소스아이디를 전달하여 핸들을 가져온다
mapFragment.getMapAsync(this); 
//getMapAsync가 메인 스레드에서 호출되어야 콜백이 메인 스레드에서 실행됩니다. 
}

@Override
public void onMapReady(GoogleMap googleMap){
LatLng SEOUL=new LatLng(37.56,126.97);

MarkerOptions markerOptions=new MarkerOptions();
markerOptions.position(SEOUL);//마커가표시될위치
markerOptions.title("서울");//타이틀
markerOptions.snippet("한국의수도");//마커클릭시보여지는설명
googleMap.addMarker(markerOptions);

googleMap.moveCamera(CameraUpdateFactory.newLatLng(SEOUL));
googleMap.animateCamera(CameraUpdateFactory.zoomTo(10));
}
```