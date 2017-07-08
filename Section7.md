7. 안드로이드 레이아웃 살펴보기
---------------------------

• 프레임 레이아웃(FrameLayout)

	여러 레이아웃, 위젯을 하나의 레이아웃(FrameLayout) 안에, 겹쳐서 사용할 수 있다.
	맨 아래 코딩된 레이아웃, 위젯이 맨 위에 보여진다. 

• 리니어 레이아웃(LinearLayout)

	세로 또는 가로의 단일 방향으로 모든 하위 항목을 정렬하는 뷰 그룹
	- android:layout_weight 속성으로 가중치 할당할 수 있음.
	- android:orientation="vertical" //뷰를 수직으로 배치

• 릴렉티브 레이아웃(RelativeLayout)

	상대적 위치에 기반하여 뷰들을 배치하는 레이아웃

	뷰의 배치 방법은 형제 요소에 대해 상대적으로 지정하는 방법과 부모뷰 영역에 상대적인 위치를 지정하는 방법으로 나눌수 있습니다.
	ex)android:layout_below / android:layout_alignParentLeft

• 퍼센트 레이아웃(PercentLayout)

비율을 통해 뷰들을 배치하는 레이아웃.
PercentFrameLayout/ PercentRelativeLayout 이 있다.

	[구현]
	1. gradle에 compile 'com.android.support:percent:23.0.0' 추가
	2. xml
	<android.support.percent.PercentRelativeLayout
	android:layout_width="0dp"
	android:layout_height="0dp"
	android:layout_centerInParent="true"
	app:layout_heightPercent="80%"
    app:layout_widthPercent="80%" />