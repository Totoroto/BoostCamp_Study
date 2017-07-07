5.안드로이드 기본 위젯 살펴보기
--------------------------

• 텍스트뷰(TextView)
화면에 텍스트를 출력

>java.lang.Object
-android.view.View
-android.widget.TextView

	[구현]
	1.XML에 TextView 추가
	2.Java소스에서 TextView에 대한 참조 획득
	TextView tv = (TextView)view.findViewById(R.id.textview);
	cf) setText()함수 호출하여 보여질 텍스트 지정
	    tv.setText("hello world");

• 에디트텍스트(EditText)
텍스트 입력

>java.lang.Object
-android.view.View
-android.widget.TextView
-android.widget.EditText

	[구현]
	1.XML에 EditText추가
	2.Java소스에서 EditText에 대한 참조 획득
	-getText()함수 호출하여 입력한 텍스트 가져올수 있음.
	String str = et.getText().toString();
	cf) 리턴 타입이 Editable이라 타입 변환해줘야한다.

• 버튼(Button)

>java.lang.Object
-android.view.View
-android.widget.TextView
-android.widget.Button

	[구현]
	1.XML에 Button추가
	2.Java소스에서 Button에 대한 참조 획득
	3.버튼 클릭 이벤트 처리
	btn_fly.setOnClickListener(new View.OnClickListener(){...

• 이미지뷰(ImageView)

>java.lang.Object
-android.view.View
-android.widget.ImageView

화면에 이미지 출력

	[구현]
	1.res/drawable/ 에 이미지 리소스 파일 추가
	2.XML에 ImageView추가
	android:src="@drawable/ic_zigu"
	3.Java소스에서 ImageView에 대한 참조 획득
	cf) 자바 소스에서 이미지 변경하기
	iv.setImageResource(R.drawable.ic_zigu);

• 체크박스(CheckBox)

>java.lang.Object            //이 아래부터 생략하겠음.
-android.view.View
-android.widget.TextView
-android.widget.Button
-android.widget.CompoundButton
-android.widget.CheckBox

    [구현] //이 아래부터 1.2.는 동일하므로 생략하겠음.
	1.checked/unchecked는 원래 자동으로 처리되도록 구현되었음.
	  하지만 CheckBox의 선택여부에 따라 동적 변화가 있을 때는 클릭 이벤트
	  리스너를 달아서 따로 구현해 주어야함.
	cf) isChecked() : checked/unchecked 여부.
	    setChecked() : CheckBox의 선택 상태를 설정할수 있음.

• 토글버튼(ToggleButton)

>android.view.View
-android.widget.TextView
-android.widget.Button
-android.widget.CompoundButton
-android.widget.ToggleButton

두 개의 상태(On/Off)를 표시하는 버튼

	[구현]
	1. 필요 시 클릭 리스너를 달아 따로 구현하면 된다.
	cf)
	-XML에서 android:textOff/ android:textOn으로 토글버튼이
	 ON/OFF되었을 때 텍스트 값을 출력해 줄 수 있다.
	-setOnCheckedChangeListener() : Change여부에 따라 구현 가능.

• 스위치(Switch)
>android.view.View
-android.widget.TextView
-android.widget.Button
-android.widget.CompoundButton
-android.widget.Switch

	토글 버튼과 비슷한데 모양만 다름.구현도 비슷하게 하면 됌.

• 라디오버튼(RadioButton)
>android.view.View
-android.widget.TextView
-android.widget.Button
-android.widget.CompoundButton
-android.widget.RadioButton

	보통 RadioGroup으로 라디오버튼을 묶어서 하나만 선택할 수 있게 구현.
	위의 토글버튼, 스위치와 구현 비슷하게 하면 됌.

• 스피너(Spinner)
>android.view.View
-android.view.ViewGroup
-android.widget.AdapterView<T extends android.widget.Adapter>
-android.widget.AbsSpinner
-android.widget.Spinner

드롭 다운으로 여러 개 아이템 중 하나를 선택할 수 있는 뷰.

	[구현]
	1. 아이템이 픽스 된 경우, /res/values폴더에 array.xml파일을 만들어서 구현가능.
	<?xml version="1.0" encoding="utf-8"?>
	<resources>
	    <string-array name="spinArray">
	        <item>Mercury</item>
	         …
	2.레이아웃에 정의된 스피너와 array.xml 연결하는 어댑터 구현
	cf)아이템을 배열에서 사용할 수 있으면 ArrayAdapter,
       db쿼리에서 사용하면 CursorAdapter

	String[] str_spin = getResource().getStringArray(R.array.spinArray);
	ArrayAdapter<String> adapter = ArrayAdapter.createFromResource(this, android.R.layout.simple_spinner_dropdown_item, str_spin);
	adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
	spinner.setAdapter(adapter);

	3. setOnItemSelectedListener() 사용하여 추가적인 부분 구현.

• 시크바(SeekBar)

>android.view.View
-android.widget.ProgressBar
-android.widget.AbsSeekBar
-android.widget.SeekBar

사용자의 터치로 상태를 변경할 수 있는 뷰.

	[구현]
	1. setOnSeekBarChangeListener() 사용하여 추가적인 부분 구현.

• 카드뷰(CardView)

>android.view.View
-android.view.ViewGroup
-android.widget.FrameLayout
-android.support.v7.widget.CardView

카드 모양의 뷰. RecyclerView와 함께 사용하여 RecyclerView의 아이템이 차지하는 뷰의 공간을 CardView로 붙여 사용할 수 있다.

	[구현]
	1. Gradle 추가 //compile 'com.android.support:cardview-v7:24.2.1'

	2. XML에 카드뷰 추가
	-카드 뷰의 부모 속성에 추가
	 xmlns:card_view="http://schemas.android.com/apk/res-auto"

	-카드뷰를 터치 가능하게 만들어주는 속성
	 android:clickable = "true"
	 android:foreground="?android:attr/selectableItemBackground"

	3. XML에서 속성을 주거나 자바 코드에서 속성 값을 지정할 수 있음
	 cardElevation :그림자가 있는 카드를 생성
	 cardCornerRadius :레이아웃에 모서리 반지름 설정
	 cardBackgroundColor :카드의 배경색을 설정

• 자동완성 텍스트뷰(AutoCompleteTextView)

>android.view.View
-android.widget.TextView
-android.widget.EditText
-android.widget.AutoCompleteTextView

몇 글자를 입력했을 때 보여줄지를 지정하는 속성과 리스트 형태로 보여줄 문자열이 미리 정의되어 있어야 한다.

	[구현]
	1.XML
	 android:completionThreshold= "2" //2글자 입력했을 때 보여준다.
	 android:completionHint="pick a number" //하단에 표시되는 문자열

	2.어댑터 구현
	String[] items = {"hello","hi"};
	ac_tv.setAdapter(new ArrayAdapter<String(this,
	android.R.layout.simple_dropdown_item_1line, items)); }

• 멀티 자동완성 텍스트뷰(MultiAutoCompleteTextView)

>android.view.View
-android.widget.TextView
-android.widget.EditText
-android.widget.AutoCompleteTextView
-android.widget.MultiAutoCompleteTextView

자동완성 텍스트뷰와 달리 여러 개의 단어를 자동으로 완성시켜 주고 콤마(,)로 단어를 구분해주는 CommaTokenizer() 구현을 해줘야한다. 나머지는 비슷.

	[구현]
	String[] items = {"hello","my","name","is"};
	edit.setTokenizer(new MultiAutoCompleteTextView.CommaTokenizer());
    edit.setAdapter(new ArrayAdapter<String>(this,android.R.layout.simple_dropdown_item_1line, items));
