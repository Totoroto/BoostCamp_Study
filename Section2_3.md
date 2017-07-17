3. 툴바 / 프래그먼트 / 머티리얼 디자인
=================================
1. 툴바
-------
```
A Toolbar is a generalization of action bars for use within application layouts.
	-optional elements
			1.navigation button
			2.branded logo image
			3.title & subtitle
			4.one or more custom view
			4.action menu
```


######	[기본 구현]
```
	1. 액션바 없애기 "Theme.AppCompat.Light.NoActionBar"
	2. xml에 툴바 선언
	3. java 코드에서 셋해주기 
	setSupportActionBar((Toolbar) findViewById(R.id.toolbar));

	cf) AppbarLayout
```
######	[+ 메뉴 만들기]
```
	1. res폴더에 메뉴 만든 후 아이템 추가하기
	<item
	android:id="@+id/menu_main"
	android:icon="@mipmap/ic_launcher"
	android:title="menu"
	app:showAsAction="always">
	
	2. 메인 액티비티에서 메뉴 생성해주기
	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
	getMenuInflater().inflate(R.menu.menu_main, menu);
	return true;
	}
	
	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
	switch (item.getItemId()){
	case R.id.first:
	Toast.makeText(getApplicationContext(), "hi",Toast.LENGTH_SHORT).show();
	break;
	//...
	}
	return true;
	}
```

2. 프래그먼트
------------

Fragment는 Activity 내에 생성되는, UI 구성을 여러 개의 모듈 단위로 작성할 수 있도록 해주는 기능입니다. 또한 한번 작성된 Fragment는 여러 Activity에서 재사용이 가능하므로 UI 구성에 소요되는 작업량을 많은 부분 감소시킬 수 있습니다.

+프래그먼트는 UI가 있어도, 없어도 상관 없다.

3. 프래그먼트를 활용한 화면 구성  // 버튼 누르면 프래그먼트 교체하는 예제 (fragment transaction)
---------------------------------------------------------------------

######1. 프래그먼트 클래스 만들기 extends Fragment
```java
	public fragmentA(){}

	@Override
	public View onCreateView(LayoutInflater inflater,@Nullable ViewGroup container,Bundle savedInstanceState){
	return inflater.inflate(R.layout.fragment_a,container,false);
	}
```
######	2. 프래그먼트 이동
```java
	FragmentManager fm=getFragmentManager();
	FragmentTransactionfragmentTransaction=fm.beginTransaction();
	
	//레이아웃 xml을 통해 Fragment 뷰 그룹에 추가 
	//add로 Fragment를 추가 하게되면 View가 계속 쌓인 만큼 느려짐..
	//id : fragmentBorC <-framelayout id
	fragmentTransaction.add(R.id.fragmentBorC,newfragmentB());
	fragmentTransaction.commit();
```
```java
private void switchFragment(){
Fragment fr;

if(isFragmentB)
	fr = new fragmentB();
else
	fr= new fragmentC();

isFragmentB=(isFragmentB) ? false : true;

FragmentManager fm = getFragmentManager();
FragmentTransaction fragmentTransaction=fm.beginTransaction();
fragmentTransaction.replace(R.id.fragmentBorC,fr);
fragmentTransaction.commit();
}
```
```
cf) 액티비티 ↔ 프래그먼트
* 액티비티 → 프래그먼트 
getFragmentManager().findFragmentById(프래그먼트 아이디);
* 프래그먼트 → 액티비티
getActivity.findViewById(액티비티 뷰);
```

4. CoordinatorLayout, CollapsingToolbarLayout
--------------------------------------------
######1. CoordinatorLayout
Behavior를 사용하기 위해서는 CoordinatorLayout을 통해서 사용되는데, CoordinatorLayout은 자식뷰의 스크롤의 변화 상태를 다른 자식뷰들에게 전달 해주는 역할을 합니다. 좀더 쉽게 말해 RecyclerView등의 스크롤의 상태를 판단하여 정의된 반응을 하기위한 View에 Behavior를 등록하면 됩니다.
http://www.kmshack.kr/tag/coordinatorlayout/

######2. CollapsingToolbarLayout
CollapsingToolbarLayout는 FrameLayout을 상속받은 Layout으로 
Toolbar를 애니메이션을 통해 확장해 주는 용도로 사용하고 있습니다.
http://iw90.tistory.com/228

5. 머티리얼 디자인
---------------
http://davidlab.net/google-design-ko/material-design/introduction.html
