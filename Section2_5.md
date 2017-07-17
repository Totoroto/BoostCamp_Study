5. 안드로이드 저장소 살펴보기
==========================

1. 프리퍼런스(Preference) 활용
---------------------------
#####SharedPreference
SharedPreference는 Map구조인 key-value 형태로 데이터를 저장합니다.
저장된 데이터는 어플리케이션이 삭제되기 전까지 내부에 파일형태로 보관됩니다.
ex) 자동로그인에 활용
```java
SharedPreferences pref;
SharedPreferences.Editor editor;
```
- 데이터 저장하기
```java
pref=getSharedPreferences("Sample",Activity.MODE_PRIVATE);
//추후Sample파일에서데이터를불러온다
editor=pref.edit();
editor.putInt("Sample",3253);
editor.commit();//변경사항저장
```
- 데이터 불러오기
```java
pref=getSharedPreferences("Sample",Activity.MODE_PRIVATE);
int num=pref.getInt("Sample",100);//오류가발생하면초기값은100으로한다
```
- 데이터 삭제하기
```java
1) editor.remove("Sample"); //방법1:key(Sample)에저장된값을삭제
2) editor.clear(); //방법2:저장된모든data삭제
```

2. 파일 활용
-----------
#####1)내부 저장소
기기 내부 저장소에 저장된 파일은 해당 애플리케이션의 전용 파일이며 다른 애플리케이션(및 사용자)은 해당 파일에 액세스할 수 없습니다
#####2) 외부 저장소
외부 저장소에 저장된 파일은 누구든지 읽을 수 있으며, 컴퓨터에서 파일을 전송하도록 USB 대용량 저장소를 사용하는 경우 사용자가 수정할 수 있습니다.

######[내부 저장소에서의 파일 활용]
```java
/*파일 쓰기*/
try{
FileOutputStream fos=openFileOutput(FILENAME,Context.MODE_PRIVATE);
//param2:파일을생성하여해당파일을여러분의애플리케이션에대해전용으로만듭니다
fos.write(str.getBytes());
fos.close();
}catch(IOExceptione){
e.printStackTrace();
}
```
```java
/*파일 읽기*/
try{
FileInputStream fis=openFileInput(FILENAME);
byte[] buffer=new byte[40];
fis.read(buffer);

String str=new String(buffer);
tv.setText(str);
fis.close();
}catch(IOExceptione){
e.printStackTrace();
}
```

3. 데이터베이스(SQLite) 활용
-------------------------
######1. 계약(Contract) 클래스를 만들어서 테이블 정의와 간단한 쿼리 정의해주기
```java
public final class DBContract{
public DBContract(){}

public static final String TABLE_NAME="SAMPLE";
public static final String COL_STUDENT_NO="number";
public static final String COL_STUDENT_NAME="name";

public static final String SQL_CREATE_SAMPLE=
"CREATE TABLE IF NOT EXISTS"+TABLE_NAME+"("+
COL_STUDENT_NO + "TEXT NOT NULL,"+
COL_STUDENT_NAME+ "TEXT" +")";

public static final String SQL_DROP_SAMPLE=
"DROP TABLE IF EXISTS"+TABLE_NAME;

public static final String SQL_SELECT="SELECT* FROM"+TABLE_NAME;

public static final String SQL_INSERT="INSERT INTO"+TABLE_NAME+" "
+"("+COL_STUDENT_NO+","+COL_STUDENT_NAME+" ) VALUES";

public static final String SQL_DELETE="DELETE FROM"+TABLE_NAME;
}
```
######2. SQLiteOpenHelper를 상속하는 클래스 만들어주기
```java
public class DBHelper extends SQLiteOpenHelper{
public static final int DATABASE_VERSION=1;
public static final String DATABASE_NAME="Sample1.db";

public DBHelper(Context context){
super(context,DATABASE_NAME,null,DATABASE_VERSION);
}

@Override
public void onCreate(SQLiteDatabase db){
db.execSQL(DBContract.SQL_CREATE_SAMPLE);
}

@Override
public void onUpgrade(SQLiteDatabase db,int oldVersion,int newVersion){
db.execSQL(DBContract.SQL_DROP_SAMPLE);
onCreate(db);
}
}
```
######3. 메인 액티비티 구현
```java
DBHelper dbHelper;
SQLiteDatabase db;
```
(1) 디비 create 해주기
```java
dbHelper=new DBHelper(this);
db=dbHelper.getWritableDatabase();
db.execSQL(DBContract.SQL_CREATE_SAMPLE);
```
(2) 디비에 값 insert
```java
db=dbHelper.getWritableDatabase();
String tempNo="20130000";
String tempName="짱구";

String sqlInsert=DBContract.SQL_INSERT+" ("+ "'"+tempNo+"', "+ "'"+tempName+"')";
db.execSQL(sqlInsert);
Toast.makeText(getApplicationContext(),"complete insert",Toast.LENGTH_SHORT).show();
```

(3)디비에 있는 값 읽어오기
```java
db=dbHelper.getReadableDatabase();
Cursor cursor=db.rawQuery(DBContract.SQL_SELECT,null);

if(cursor.moveToFirst()){
//primarykey인학번가져오기
String Snum=cursor.getString(0);
//이름가져오기
String Sname=cursor.getString(1);

tv.setText("학번:"+Snum+"|이름:"+Sname);
}
```
(4)디비에 있는 값 삭제하기
```java
db=dbHelper.getWritableDatabase();
db.execSQL(DBContract.SQL_DELETE);
```