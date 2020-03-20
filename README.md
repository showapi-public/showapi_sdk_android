# showapi_sdk_android
showapi_sdk_android


| 方法名 | 说明| 参数|
|  -- | -- | -- |
| addTextPara | 向请求中添加一个字符串类型的请求参数| 第一个参数:参数名,第二个参数: 字符串|
| addFilePara | 向请求中添加一个File类型的请求参数| 第一个参数:参数名,第二个参数: File对象|
| post | 以post的方法,发送参数到url地址,返回json字符串| |

调用接口前确保项目可以接通网络,如果用到参数类型为文件的参数,还需要获得读取文件的权限

## 	普通POST   demo
   ```html
//以下代码为纯java实现，并未依赖第三方框架,具体传入参数请参看接口描述详情页.
protected Handler mHandler =  new Handler();
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);
final TextView txt = (TextView) this.findViewById(R.id.textView1);
Button myBtn = (Button) this.findViewById(R.id.button1);
myBtn.setOnClickListener(new OnClickListener() {
public void onClick(View v) {
new Thread(){
//在新线程中发送网络请求
public void run() {
String appid="xxx";//要替换成自己的
String secret="xxxxxxx";//要替换成自己的
final String res=new ShowApiRequest( "http://route.showapi.com/64-19", appid, secret)
    .addTextPara("com", "zhongtong")
    .addTextPara("nu", "75312165465979")
    .addTextPara("senderPhone", "")
    .addTextPara("receiverPhone", "")
.post();
System.out.println(res);
//把返回内容通过handler对象更新到界面
mHandler.post(new Thread(){
public void run() {
txt.setText(res+"  "+new Date());
}
});
}
}.start();


}
});
}

   ```

## 	文件POST   demo
   ```html
//以下代码为纯java实现，并未依赖第三方框架,具体传入参数请参看接口描述详情页.
protected Handler mHandler =  new Handler();
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);
final TextView txt = (TextView) this.findViewById(R.id.textView1);
Button myBtn = (Button) this.findViewById(R.id.button1);
myBtn.setOnClickListener(new OnClickListener() {
public void onClick(View v) {
new Thread(){
//在新线程中发送网络请求
public void run() {
String appid="xxx";//要替换成自己的
String secret="xxxxxxx";//要替换成自己的
final String res=new ShowApiRequest( "http://route.showapi.com/1129-2", appid, secret)
				.addFilePara("imgFile", new File("替换为你的文件"))
.post();
System.out.println(res);
//把返回内容通过handler对象更新到界面
mHandler.post(new Thread(){
public void run() {
txt.setText(res+"  "+new Date());
}
});
}
}.start();


}
});
}
   ```

## base64处理   demo
```html
//具体传入参数请参看接口描述详情页.
       @Override
          protected void onCreate(Bundle savedInstanceState) {
              super.onCreate(savedInstanceState);
              setContentView(R.layout.activity_main);
              new Thread(){
                  @Override
                  public void run() {
                      File file = new File("/sdcard/20200317161547.png");//需要转换的文件
                      String str = encodeBase64File(file); //把文件转换为base64 ,在文件转base64之前需要获得读取该文件的权限,否则会抛出异常
                      String appid="xxx";//要替换成自己的
                      String secret="xxxxxxx";//要替换成自己的
                      String st = new ShowApiRequest("https://route.showapi.com/1129-4",appid,secret)//调用接口
                              .addTextPara("imgData",str)
                              .post();
                      System.out.println(st);
                  }
              }.start();
          }

//将文件转换为base64
public  String encodeBase64File(File file){
        byte[] buffer = null;
        try{
            FileInputStream fis = new FileInputStream(file);
            ByteArrayOutputStream bos = new ByteArrayOutputStream();
            byte[] b = new byte[1024];
            int n;
            while ((n = fis.read(b)) != -1){
                bos.write(b, 0, n);
            }
            fis.close();
            bos.close();
            buffer = bos.toByteArray();
        }catch (FileNotFoundException e){
            e.printStackTrace();
        }catch (IOException e){
            e.printStackTrace();
        }
         return new String(Base64.encode(buffer, Base64.NO_WRAP));//(import android.util.Base64;)base64需要导包
    }

```


