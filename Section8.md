8. 위젯의 이벤트 처리하기
----------------------
• 버튼 클릭 처리하기
1. OnClickListener를 생성과 동시에 만드는 방법
```
	btn.setOnClickListener(new View.OnClickListener() {
		@Override
	   	public void onClick(View v) {
	                //To do
	    }
    });
```
2. 액티비티에 OnClickListener 인터페이스를 받아 버튼을 연결
```
public class MainActivity extends AppCompatActivity implements View.OnClickListener{

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        ...
	        Button btn = (Button)findViewById(R.id.button);
	        btn.setOnClickListener(this);
	    }

	    @Override
	    public void onClick(View v) {
	        //TO DO
	    }
```
	3.OnClickListener 따로 만들어 구현
```
	public class MainActivity extends AppCompatActivity{

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        ...
	        Button btn = (Button)findViewById(R.id.button);

	        btn.setOnClickListener(mClickListener);
	    }

	    Button.OnClickListener mClickListener = new View.OnClickListener() {
	        @Override
	        public void onClick(View v) {
	            //To do
	        }
	    };
```
4. XML에 android:OnClick = "btnStart"
메인 액티비티에서 public void btnStart(View v) {...} 메소드 구현


• 위젯 터치 이벤트 처리하기

1.리스너 인터페이스 구현 boolean onTouch(View v, MotionEvent event)

리스너는 여러 대상에 대해 등록이 가능하기 때문에 이벤트 대상 뷰인 v를 전달 받아야 한다.
```
	tv.setOnTouchListener(new View.OnTouchListener() {
	            @Override
	            public boolean onTouch(View view, MotionEvent motionEvent) {
	                Toast.makeText(getApplicationContext(), "touch", Toast.LENGTH_LONG).show();
	                return false;
	            }
	        });
```
2.콜백 메소드 정의 boolean onTouchEvent(MotionEvent event)

콜벡 메서드는 해당 뷰에서 재정의 하므로 이벤트 발생 대상이 정해져있다.
```
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

	    View v = new MyView(this);
	    setContentView(v);
	}

	protected class MyView extends View {
		public MyView(Context context) {
	    	super(context);
	    }

	public boolean onTouchEvent(MotionEvent event) {
		super.onTouchEvent(event);

	    if (event.getAction() == MotionEvent.ACTION_DOWN) {
        	Toast.makeText(getApplicationContext(), "down", Toast.LENGTH_SHORT).show();
	    	return true;
	    }

	return false;
	}
}
```

• 그 밖의 다양한 이벤트 종류 살펴보기

| 이벤트 발생 시점 | 이벤트 리스너의 이름 |
|--------|--------|
|뷰가 윈도우에 탈착(붙었다 떨어졌다 함)될 때 |OnAttachStateChangeListener|
|뷰를 클릭할 때 (대표 사례: Button) |OnClickListener |
|뷰를 길게 클릭할 때 | OnLongClickListener
|컨텍스트 메뉴가 생성될 때|OnCreateContextMenuListener|
|뷰를 드래그할 때 |OnDragListener|
|포커스가 한 뷰에서 다른 뷰로 바뀔 때 |OnFocusChangeListener |
|제네릭 모션이 일어날 때 |OnGenericMotionListener |
|하버 이벤트가 일어날 때| OnHoverListener |
|키를 누르거나 뗄 때 |OnKeyListener|
|레이아웃 처리로 인해 뷰의 레이아웃 경계(layout bounds)가 바뀔 때| OnLayoutChangeListener|
|상태 바(status bar)가 가시성(visibility)을 바꿀 때|OnSystemUiVisibilityChangeListener
|사용자가 화면에 뷰를 터치할 때 (손을 댈 때, 댄 채로 움직일 때, 뗄 때 모두 포함)|OnTouchListener
