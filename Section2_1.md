2-1. 알림 기능 살펴보기
===============
1. 토스트
----
```java
	Toast.makeText(Context context, String msg, int duration)
	param1 : Context 클래스를 상속한 액티비티
	param2 : 보여주고 싶은 메시지
	param3 : 디스플레이 시간
```
-토스트 위치 바꾸기
```java
	void setGravity(int gravity, int xOffset, int yOffset)
	토스트 뷰가 보이는 화면 상의 위치를 지정하는 메소드

	param1 : 정렬 위치 지정 ex)Gravity.CENTER
	param2(-160dp~160),param3(-240~240) : 입력한 값만큼 토스트가 x,y로 이동

	[Example]
    toast.setGravity(Gravity.CENTER_HORIZONTAL | Gravity.CENTER_VERTICAL, 0, 0);

	void setMargin(float horizontalMargin, float verticalMargin)
	외부 여백을 지정해주는 메소드, 이 값을 이용해 토스트를 중앙이나 우측 하단에 배치 가능

	void cancel() : 다른 토스트가 뜨는 것을 방해, but 본래 지속시간 유지
	void Toast.show() : 생성한 토스트를 보여줌
```
-토스트 색깔 바꾸기
```java
	LayoutInflater inflater = getLayoutInflater(); //xml에 정의된 레이아웃을 객체화
	View layout = inflater.inflate(R.layout.toast_border, (ViewGroup) findViewById(R.id.toastLayoutRoot));

	Toast toast = Toast.makeText(getApplicationContext(),"hello",Toast.LENGTH_LONG);
	toast.setView(layout);
	toast.show();
```
2. 스낵바
---
```java
	Snackbar.make(View view, CharSquence text, int duration)
```
[토스트와의 차이]
- Snackbar은 Action을 통해 OnClick을 설정할 수 있다.
- Toast는 Context를 인자로 받고, Snackbar는 View를 매개변수로 받는다.

[기본 코드]
```java
  View view = getWindow().getDecorView().getRootView(); //액티비티 뷰
  Snackbar.make(view, "hi",Snackbar.LENGTH_SHORT)
  .setAction("OK", new View.OnClickListener() {
  	@Override
      public void onClick(View view) {
          tv.setText("스낵바 클릭 확인");
      }}).show();
```
* 스낵바 색깔 바꾸기
	1) 텍스트 컬러
```java
Snackbar.make(view,"hi",Snackbar.LENGTH_SHORT).setActionTextColor(Color.CYAN)
```
	2) 백그라운드 컬러
```java
	View snackView = snackbar.getView();
	snackView.setBackgroundColor(Color.parseColor("#999999"));
```
* 스낵바 위치 바꾸기
```xml
	[xml]
	<android.support.design.widget.CoordinatorLayout
	        android:id="@+id/cl"
	        android:layout_width="match_parent"
	        android:layout_height="wrap_content"
	        android:layout_alignParentTop="true"
	        >
	    </android.support.design.widget.CoordinatorLayout>
```
```java
    [java]
	View viewPos = findViewById(R.id.cl);
	Snackbar snackbar = Snackbar.make(viewPos,"hi",Snackbar.LENGTH_SHORT);
```

3. 대화상자(Dialog)
--------------
Dialog 클래스가 대화상자의 기본 클래스이지만, Dialog를 직접 인스턴스화하는 것은 삼가야 합니다.

1) AlertDialog
	제목 하나, 최대 세 개의 버튼, 선택 가능한 품목 목록 또는 사용자 지정 레이아웃을 표시할 수 있는 대화상자입니다.
```java
	AlertDialog dialog = createBox();
	dialog.show();

	private AlertDialog createBox() {
	AlertDialog.Builder builder = new AlertDialog.Builder(this);

	builder.setTitle("제목");
	builder.setMessage("종료하시겟습니까");
	builder.setIcon(R.mipmap.ic_launcher);

	builder.setPositiveButton("좋아요", new DialogInterface.OnClickListener() {..
	builder.setNeutralButton("취소", new DialogInterface.OnClickListener() {..
	builder.setNegativeButton("아니오", new DialogInterface.OnClickListener() {..
```

	cf) DatePickerDialog 또는 TimePickerDialog
	미리 정의된 UI가 있는 대화상자로 사용자로 하여금 날짜 또는 시간을 선택할 수 있게 해줍니다.
	이러한 클래스가 대화상자의 스타일과 구조를 정의하지만, 대화상자의 컨테이너로는 DialogFragment를 사용해야 합니다.

EXAMPLE
	[DatePickerFragment.java]
```java
	public class DatePickerFragment extends DialogFragment
	        implements DatePickerDialog.OnDateSetListener {
            
	    @Override
	    public Dialog onCreateDialog(Bundle savedInstanceState) {
	        Calendar c = Calendar.getInstance();
	        int year = c.get(Calendar.YEAR);
	        int month = c.get(Calendar.MONTH);
	        int day = c.get(Calendar.DAY_OF_MONTH);
	
	        return new DatePickerDialog(getActivity(), this, year, month, day);
	    }
	
		@Override
	    public void onDateSet(DatePicker datePicker, int y, int m, int d){
	        TextView tv = (TextView)getActivity().findViewById(R.id.tv);
	        Toast.makeText(getActivity(),"아까선택한날",Toast.LENGTH_LONG).show();
	        tv.setText(String.valueOf(y+"/"+m+"/"+d));
	    }
```
```java
[MainActivity.java] implements DatePickerDialog.OnDateSetListener

	tv.setOnClickListener(new View.OnClickListener() {
	       @Override
	       public void onClick(View view) {
	            DialogFragment dialogFragment = new DatePickerFragment();
	            dialogFragment.show(getSupportFragmentManager(), "DATEPICKER");
	       }
	 });
```

	2)DialogFragment
	API 8 부터는 Fragment 의 등장으로, dialog 를 DialogFragment 로 대체하려 함.
	DialogFragment는 dialog 의 생명주기를 dialog 자체에 함수콜을 하는 형태가 아니라, system 에 어느 정도 맡기고 우회해서 call 하는 형태.
```java
	[MyDialog.java]

	public class MyDialog extends DialogFragment {

	    @NonNull
	    @Override
	    public Dialog onCreateDialog(Bundle savedInstanceState) {
	        AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
	        builder.setMessage("메시지 내용을 보여줍니다")
	               .setPositiveButton("네", ...)
	                .setNegativeButton("아니오", ...);
	        return builder.create();
	    }
	}
```
```java
	[MainActivity.java]

	MyDialog myDialog = new MyDialog();
	myDialog.show(getSupportFragmentManager(), "sysTag"); //매니저 객체&fragment tag
```

- 다이얼로그 ↔ 액티비티
```java
	[MyDialog.java]
		//1.리스너 인터페이스 선언
	    public interface MyDialogFragmentListener{
	        public void onDialogClick(DialogFragment d, String str);
	    }
	    //2.리스너 객체 생성
	    MyDialogFragmentListener myDialogFragmentListener;

	    //3.리스너 인터페이스와 액티비티를 연결
	   @Override
	    public void onAttach(Context context) {
	        super.onAttach(context);

	       myDialogFragmentListener = (MyDialogFragmentListener) context;
	    }

	    //4.리스너 인터페이스 속의 메소드 작동 + 연결된 액티비티와 통신
	    public void clickAction(){
	        //To do...
	        myDialogFragmentListener.onDialogClick(MyDialog.this, "Success");
	    }
```
```java
[MainActivity.java] extends MyDialogFragmentListener
	 btn.setOnClickListener(new View.OnClickListener() {
	    @Override
	    public void onClick(View view) {
	       myDialog.clickAction();
	    }
	});
	        
	@Override
	public void onDialogClick(DialogFragment d, String str) {
	//To do...
	  tv.setText(str);
	  d.dismiss();
	}
```	

3) 기타 옵션
- 여러 개 멀티 선택 옵션
		 builder.setSingleChoiceItems(array, 0, new DialogInterface.OnclickListener.. 
- HTML로 구현한 텍스트 넣기
		 builder.setMessage(Html.fromHtml("…."));
- ProgressiveDialog(로딩중? 다이얼로그)
```
ProgressDialog dialog = ProgressDialog.show(DialogSample.this, "", "기다려 주세요 ...", true);
```
4. 노티피케이션
------------
알림에는 자체적인 디자인 지침이 있습니다. 고로 머티리얼 디자인 참고!
Notification 객체는 다음을 반드시 포함해야 합니다.

1. setSmallIcon()이 설정한 작은 아이콘
2. setContentTitle()이 설정한 제목
3. setContentText()이 설정한 세부 텍스트

[기본적인 노티피케이션]
```java
private void aboutBasicNotification() {

NotificationManager notificationManager = (NotificationManager)getSystemService(Context.NOTIFICATION_SERVICE);

Intent intent = new Intent(this, MainActivity.class);
//intent.putExtra("number", 3253);

	        /**
	         * PendingIntent 알림을 터치하면 실행할 액티비티(또는 서비스)를 지정
	         *
	         * FLAG_CANCEL_CURRENT : 이전에 생성한 PendingIntent 는 취소하고 새롭게 만듭니다
	         * FLAG_NO_CREATE : 생성된 PendingIntent를 반환하여 재사용
	         * FLAG_ONE_SHOT : 이 flag 로 생성한 PendingIntent는 일회용
	         * FLAG_UPDATE_CURRENT : 이미 생성된 PendingIntent가 존재하면 내용을 변경합니다
	         */

PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);

Notification.Builder builder = new Notification.Builder(this); //builder :알림에 대한 UI 정보와 작업을 지정

builder.setPriority(Notification.PRIORITY_HIGH); //priority가 높으면 한 줄 메시지가 아니라 크게 보임.

builder.setLargeIcon(BitmapFactory.decodeResource(getResources(),R.mipmap.ic_launcher)); //우측 큰 아이콘

builder.setSmallIcon(android.R.drawable.star_on); //!!!프로젝트명 좌측 작은 아이콘
	
builder.setTicker("알람 발생시 잠깐 나오는 텍스트");
builder.setContentTitle("알람 주제목"); //!!!
builder.setContentText("알람 부제목"); //!!!

builder.setWhen(System.currentTimeMillis()); //알람시간, System.currentTimeMillis() : 지금!

builder.setDefaults(Notification.DEFAULT_SOUND | Notification.DEFAULT_VIBRATE); //알람 시 진동or사운드 설정
builder.setContentIntent(pendingIntent); //알람 눌렀을 때 실행할 인텐트
builder.setAutoCancel(true); //true : 알람 터치 시 자동으로 삭제
```

[Big Picture Style]
	노티피케이션을 잡아 밑으로 내리면 이 큰 그림이 보여요 ex)스크린샷
```java
builder.setStyle(new Notification.BigPictureStyle()
	   .bigPicture(BitmapFactory.decodeResource(getResources(),R.mipmap.ic_launcher)));
```
[Big Text Style]
노티피케이션을 잡아 밑으로 내리면 긴 메시지가 보여요 ex)SMS
```java
builder.setStyle(new Notification.BigTextStyle()
	   .bigText("여자친구!\n" +"미처 말하지 못했어 다만 너를 좋아했어\n" +
	   "어린 날의 꿈처럼 마치 기적처럼\n" +
	   "시간을 달려서 어른이 될 수만 있다면\n" +
	   "거친 세상 속에서 손을 잡아 줄게"));
```

[InBox Style]
노티피케이션을 잡아 밑으로 내리면 추가된 string이 보여요. ex)Gmail
```java
builder.setStyle(new Notification.InboxStyle()
	            .addLine("뭔가 더 보여진다")
	                .addLine("GMAIL이랑 비슷하다")
	                .setSummaryText("+5 MORE")
	        );
```

[노티피케이션 업데이트하기]
```java
mNotificationManager = (NotificationManager)getSystemService(Context.NOTIFICATION_SERVICE);

NotificationCompat.Builder builder = new NotificationCompat.Builder(this)
	        .setContentTitle("New Message")
	        .setContentText("You've received new messages.")
	        .setSmallIcon(R.drawable.ic_unchecked);
	
	        builder.setContentText("HI HU HE HO");
	        // Because the ID remains unchanged, the existing notification is updated.
		// Sets an ID for the notification, so it can be updated
	        mNotificationManager.notify(0, builder.build());
```
[알림 없어지는 경우]
```
	1. 사용자가 알림을 클릭하고, 알림을 생성했을 때 setAutoCancel() 을 호출했을 경우
	2. 특정 알림 ID에 대해 cancel()을 호출합니다. 이 메서드도 현재 진행 중인 알림을 삭제
	3. cancelAll()을 호출합니다. 이것은 이전에 발행한 알림을 모두 제거.
```
[노티피케이션에 프로그레스 바 표시하기]
```java
	new Thread(
	    new Runnable() {
	    @Override
	      public void run() {
	          int incr;
	          // Do the "lengthy" operation 20 times
	          for (incr = 0; incr <= 100; incr+=5) {
	                mBuilder.setProgress(100, incr, false);
// (바의 최대값, 현재 프로그레스 값, true : 대기중/ false : 프로그레스 값 적용)
	                mNotifyManager.notify(0, mBuilder.build());

	                   try {
	                      Thread.sleep(5*1000); // Sleep for 5 seconds
	                    } catch (InterruptedException e) {
	                      Log.d(TAG, "sleep failure");
	                   }
	          }
	 // When the loop is finished, updates the notification
	          mBuilder.setContentText("Download complete")
	                  .setProgress(0,0,false); // Removes the progress bar
	          mNotifyManager.notify(1, mBuilder.build());
	   }
	}).start();
```
[커스텀 노티피케이션]
```java
NotificationManager notificationManager = (NotificationManager)getSystemService(Context.NOTIFICATION_SERVICE);
	
	 RemoteViews contentView = new RemoteViews(getPackageName(), R.layout.layout_notification);
	 
     contentView.setImageViewResource(R.id.image, R.mipmap.ic_launcher);
	 contentView.setTextViewText(R.id.title, "Custom notification");
	 contentView.setTextViewText(R.id.text, "This is a custom layout");
	
	 NotificationCompat.Builder mBuilder = new NotificationCompat.Builder(this)
	                .setSmallIcon(R.mipmap.ic_launcher)
	                .setContent(contentView)
	                .setDefaults(Notification.DEFAULT_SOUND | Notification.DEFAULT_VIBRATE) //알람 시 진동or사운드 설정
	                .setAutoCancel(true);
	        
     notificationManager.notify(1, mBuilder.build());
```