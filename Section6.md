6. 안드로이드 뷰 컨테이너 살펴보기
-----------------------------
• 스크롤뷰(ScrollView)
세로 방향의 스크롤.
스크롤 할 수 없는 뷰가 스크롤 될 수 있게 해주는 컨테이너.
스크롤 뷰 안에는 오직 한 개의 뷰만 들어갈 수 있다.
원하는 부분만 ScrollView로 감싸면 된다.

[속성]
android:fillViewport : 하위 뷰의 match_parent 적용 가능하게 한다.
android:scrollbars : 스크롤 바 형식 지정

• 수평 스크롤뷰(HorizontalScrollView)
가로 방향의 스크롤.

• 뷰페이저(ViewPager)
Page를 넘기듯이 손가락으로 드래그해서 좌우로 넘기는 UI.
```
	[구현]
	1. 메인 xml에 <ViewPager> 추가
	- 프래그먼트 생성하려면, 프래그먼트 xml도 추가
	+ 프래그먼트 클래스 생성(default constructor, OnCreateView 구현)

	@Override
	public View onCreateView(LayoutInflater inflater, ViewGroup container,
    Bundle savedInstanceState) {
		return inflater.inflate(R.layout.fragment_a, container, false);
	}

	2. MainActivity 구현
	- ViewPager 객체 생성, setAdapter(뷰페이저와 어댑터 연결),
	  setCurrentItem(0) //앱 실행됐을때 첫번째 페이지로 초기화

     vp.setAdapter(new MyAdapter(getSupportFragmentManager()));
     vp.setCurrentItem(0);

	3. 어댑터 구현

	private class MyAdapter extends FragmentStatePagerAdapter {
		public MyAdapter(FragmentManager fm) {
			super(fm);
		}
		@Override
		public Fragment getItem(int position) {
		//포지션에 따라 프래그먼트를 리턴
	            switch (position){
	                case 0:
	                    return new FragmentA();
	                case 1:
	                    return new FragmentB();
	                default:
	                    return null;
	            }
	     }

		@Override
	     public int getCount() {
		//뷰페이저안에 들어가는 페이지 갯수
	         return 2;
	     }
```
* 데이트피커(DatePicker) : 날짜         cf) DatePickerDialog

	[속성]
	android:datePickerMode="spinner"
	android:calendarViewShown="false"
```
	[구현]
    init() 으로 초기값 지정
	datePicker.init(datePicker.getYear(), datePicker.getMonth(),
    datePicker.getDayOfMonth(),new DatePicker.OnDateChangedListener() {
		@Override
		public void onDateChanged(DatePicker datePicker, int y,
        int m, int d) {
		//to do}
   	 });
```
* 타임피커(TimePicker) : 시간         cf) TimePickerDialog

	데이트피커와 비슷하게 구현하면 됌.
	cf) timepicker.setOnTimeChangedListener()

* 리사이클러뷰(RecyclerView)
```
	[구현]
	1. Gradle에 compile 'com.android.support:recyclerview-v7:24.2.1' 추가
	2. xml에 RecyclerView 추가
	3. LayoutManager 정의 : xml 또는 자바 코드로 정의하면 된다.
	- XML
	<android.support.v7.widget.RecyclerView
	    app:layoutManager="GridLayoutManager"
	    app:spanCount="2" />
	- 자바 코드
	mLayoutManager = new LinearLayoutManager(this);
	mRecyclerView.setLayoutManager(mLayoutManager);
	4. RecyclerView에 담길 아이템 생성해주기.
	5. 뷰 홀더 정의
	public class GreenAdapter extends RecyclerView.Adapter<GreenAdapter.NumberViewHolder>{
	    private int mNumberItems; //몇개의 뷰를 어댑터가 만들어야하니

	    public GreenAdapter(int numberOfItems){
	        mNumberItems = numberOfItems;
	    }

	 /*
	    * RecyclerView를 ViewHolder의 객체로 인스턴스화
	    * 아이템뷰를 XML과 연결시켜 하나의 뷰를 만들어내고
	    * 그 다음 새로운 뷰를 가지는 새로운 뷰홀더 객체를 반환 */
	    @Override
	    public NumberViewHolder onCreateViewHolder(ViewGroup viewGroup, int viewType) {
	        Context context = viewGroup.getContext();
	        int layoutForListItem = R.layout.number_list_item; //XML ID
	        LayoutInflater inflater = LayoutInflater.from(context);
	        boolean shouldAttachToParentImmediately = false;
	        //부모 뷰그룹과 레이아웃이 바로 연결되지 않도록 해준다.

	        //inflater : 레이아웃 XML을 받아서 뷰로 반환
	        View view = inflater.inflate(layoutForListItem, viewGroup, shouldAttachToParentImmediately);
	        NumberViewHolder viewHolder = new NumberViewHolder(view);

	        return viewHolder;
	    }

	    @Override
	    public void onBindViewHolder(NumberViewHolder holder, int position)  {
	        holder.bind(position);
	    }

	    @Override
	    public int getItemCount() {
	        return mNumberItems; //데이터소스의 아이템갯수 반환
	    }

	    class NumberViewHolder extends RecyclerView.ViewHolder{ //커스텀 뷰홀더
	        TextView listItemNumberView;

        public NumberViewHolder(View itemView){
	            super(itemView);
	            listItemNumberView = (TextView) itemView.findViewById(R.id.tv_item_number);
	        }

	        void bind(int listIndex){
	            listItemNumberView.setText(String.valueOf(listIndex));
	        }
	    }
	}

	6. RecyclerView의 Adapter 설정 해주기.
	mNumbersList.setHasFixedSize(true);
    //true: 어댑터의 내용이바껴도 전체레이아웃이 깨지지X
	mAdapter = new GreenAdapter(NUM_LIST_ITEMS);
    mNumbersList.setAdapter(mAdapter);
```