---
title: JAVA时间总结
date: 2019-09-17 20:28:36
urlname: java_date
tags: [JAVA,DATE]
categories: JAVA
---
 
# JAVA时间总结
 
---


## 1.UTC(世界标准时间)
什么是utc时间
>即协调世界时。UTC是以原子时秒长为基础，在时刻上尽量接近于GMT的一种时间计量系统。为确保UTC与GMT相差不会超过0.9秒，在有需要的情况下会在UTC内加上正或负闰秒。UTC现在作为世界标准时间使用


## 2.GMT(格林尼治平时)
>即格林尼治标准时间，也就是世界时。GMT的正午是指当太阳横穿格林尼治子午线（本初子午线）时的时间。但由于地球自转不均匀不规则，导致GMT不精确，现在已经不再作为世界标准时间使用。

UTC和GMT时间误差不超过0.9秒,即两者基本相同

## 3.CST(北京时间)
> 北京时间，China Standard Time，中国标准时间。在时区划分上，属东八区，比协调世界时早8小时，记为UTC+8


java日期工具类：

```java
public class DateUtils {
	 private static final SimpleDateFormat datetimeFormat = new SimpleDateFormat(  
	            "yyyy-MM-dd HH:mm:ss");
    private static final SimpleDateFormat dateFormat = new SimpleDateFormat(  
            "yyyy-MM-dd");
    private static final SimpleDateFormat timeFormat = new SimpleDateFormat(  
            "HH:mm:ss");  
  
    /** 
     * 获得当前日期时间 
     * <p> 
     * 日期时间格式yyyy-MM-dd HH:mm:ss 
     *  
     * @return 
     */  
    public static String currentDatetime() {  
        return datetimeFormat.format(now());  
    }  
  
    /** 
     * 格式化日期时间 
     * <p> 
     * 日期时间格式yyyy-MM-dd HH:mm:ss 
     *  
     * @return 
     */  
    public static String formatDatetime(Date date) {  
        return datetimeFormat.format(date);  
    }  

 
    
    /** 
     * 格式化日期时间 
     *  
     * @param date 
     * @param pattern 
     *            格式化模式，详见{@link SimpleDateFormat}构造器 
     *            <code>SimpleDateFormat(String pattern)</code> 
     * @return 
     */  
    public static String formatDatetime(Date date, String pattern) {  
        SimpleDateFormat customFormat = (SimpleDateFormat) datetimeFormat  
                .clone();  
        customFormat.applyPattern(pattern);  
        return customFormat.format(date);  
    }  
    
    /**
     * 格式化 时间戳
     * @param date
     * @param pattern
     * @return
     */
    public static String formatDatetime(Timestamp date, String pattern) {  
        SimpleDateFormat customFormat = (SimpleDateFormat) datetimeFormat  
                .clone();  
        customFormat.applyPattern(pattern);  
        return customFormat.format(date);  
    }
    
    /**
     * 字符串日期 格式化后 变为字符串形式
     * @param date
     * @param pattern
     * @return
     */
    public static String formatDatetime(String date, String pattern) {  
        Date date2 = null;
		try {
			SimpleDateFormat customFormat = (SimpleDateFormat) datetimeFormat  
			        .clone();  
			customFormat.applyPattern(pattern);  
			date2 = customFormat.parse(date );
		} catch (ParseException e) {
			e.printStackTrace();
		} 
        
        return formatDatetime(date2, pattern);		
    }
 
    /**
     * 
     * @param
     * @return
     */
    public static long getCurTimestamp(Date date){
    	String curDate =  formatDatetime( date, "yyyyMMddHHmmss" ) ;
    	 
    	return Long.parseLong( curDate) ; 
    }

    /**
    * 字符串转换成日期
    * @param
    * @return pattern 日期格式 "yyyy-MM-dd HH:mm:ss"
    */
    public static Date strToDate(String dateStr,String pattern) {
      
       SimpleDateFormat format = new SimpleDateFormat(pattern);
       Date date = null;
       try {
        date = format.parse(dateStr);
       } catch (ParseException e) {
    	   e.printStackTrace();
       }
       return date;
    }
    
    /** 
     * 获得当前日期 
     * <p> 
     * 日期格式yyyy-MM-dd 
     *  
     * @return 
     */  
    public static String currentDate() {  
        return dateFormat.format(now());  
    }  
  
    /** 
     * 格式化日期 
     * <p> 
     * 日期格式yyyy-MM-dd 
     *  
     * @return 
     */  
    public static String formatDate(Date date) {  
        return dateFormat.format(date);  
    }  
  
    /** 
     * 获得当前时间 
     * <p> 
     * 时间格式HH:mm:ss 
     *  
     * @return 
     */  
    public static String currentTime() {  
        return timeFormat.format(now());  
    }  
  
    /** 
     * 格式化时间 
     * <p> 
     * 时间格式HH:mm:ss 
     *  
     * @return 
     */  
    public static String formatTime(Date date) {  
        return timeFormat.format(date);  
    }  
  
    /** 
     * 获得当前时间的<code>java.util.Date</code>对象 
     *  
     * @return 
     */  
    public static Date now() {  
        return new Date();  
    }  
  
    public static Calendar calendar() {  
        Calendar cal = GregorianCalendar.getInstance(Locale.CHINESE);  
        cal.setFirstDayOfWeek(Calendar.MONDAY);  
        return cal;  
    }  
  
    /** 
     * 获得当前时间的毫秒数 
     * <p> 
     * 详见{@link System#currentTimeMillis()} 
     *  
     * @return 
     */  
    public static long millis() {  
        return System.currentTimeMillis();  
    }  
  
    /** 
     *  
     * 获得当前Chinese月份 
     *  
     * @return 
     */  
    public static int month() {  
        return calendar().get(Calendar.MONTH) + 1;  
    }  
  
    /** 
     * 获得月份中的第几天 
     *  
     * @return 
     */  
    public static int dayOfMonth() {  
        return calendar().get(Calendar.DAY_OF_MONTH);  
    }  
  
    /** 
     * 今天是星期的第几天 
     *  
     * @return 
     */  
    public static int dayOfWeek() {  
        return calendar().get(Calendar.DAY_OF_WEEK);  
    }  
  
    /** 
     * 今天是年中的第几天 
     *  
     * @return 
     */  
    public static int dayOfYear() {  
        return calendar().get(Calendar.DAY_OF_YEAR);  
    }  
  
    /** 
     *判断原日期是否在目标日期之前 
     *  
     * @param src 
     * @param dst 
     * @return 
     */  
    public static boolean isBefore(Date src, Date dst) {  
        return src.before(dst);  
    }  
  
    /** 
     *判断原日期是否在目标日期之后 
     *  
     * @param src 
     * @param dst 
     * @return 
     */  
    public static boolean isAfter(Date src, Date dst) {  
        return src.after(dst);  
    }  
  
    /** 
     *判断两日期是否相同 
     *  
     * @param date1 
     * @param date2 
     * @return 
     */  
    public static boolean isEqual(Date date1, Date date2) {  
        return date1.compareTo(date2) == 0;  
    }  
  
    /** 
     * 判断某个日期是否在某个日期范围 
     *  
     * @param beginDate 
     *            日期范围开始 
     * @param endDate 
     *            日期范围结束 
     * @param src 
     *            需要判断的日期 
     * @return 
     */  
    public static boolean between(Date beginDate, Date endDate, Date src) {  
        return beginDate.before(src) && endDate.after(src);  
    }  
  
    /** 
     * 获得当前月的最后一天 
     * <p> 
     * HH:mm:ss为0，毫秒为999 
     *  
     * @return 
     */  
    public static Date lastDayOfMonth() {  
        Calendar cal = calendar();  
        cal.set(Calendar.DAY_OF_MONTH, 0); // M月置零  
        cal.set(Calendar.HOUR_OF_DAY, 0);// H置零  
        cal.set(Calendar.MINUTE, 0);// m置零  
        cal.set(Calendar.SECOND, 0);// s置零  
        cal.set(Calendar.MILLISECOND, 0);// S置零  
        cal.set(Calendar.MONTH, cal.get(Calendar.MONTH) + 1);// 月份+1  
        cal.set(Calendar.MILLISECOND, -1);// 毫秒-1  
        return cal.getTime();  
    }  
  
    /** 
     * 获得当前月的第一天 
     * <p> 
     * HH:mm:ss SS为零 
     *  
     * @return 
     */  
    public static Date firstDayOfMonth() {
        Calendar cal = calendar();  
        cal.set(Calendar.DAY_OF_MONTH, 1); // M月置1  
        cal.set(Calendar.HOUR_OF_DAY, 0);// H置零  
        cal.set(Calendar.MINUTE, 0);// m置零  
        cal.set(Calendar.SECOND, 0);// s置零  
        cal.set(Calendar.MILLISECOND, 0);// S置零  
        return cal.getTime();  
    }  
  
    private static Date weekDay(int week) {  
        Calendar cal = calendar();  
        cal.set(Calendar.DAY_OF_WEEK, week);  
        return cal.getTime();  
    }  
  
    /** 
     * 获得周五日期 
     * <p> 
     * 注：日历工厂方法{@link #calendar()}设置类每个星期的第一天为Monday，US等每星期第一天为sunday 
     *  
     * @return 
     */  
    public static Date friday() {  
        return weekDay(Calendar.FRIDAY);  
    }  
  
    /** 
     * 获得周六日期 
     * <p> 
     * 注：日历工厂方法{@link #calendar()}设置类每个星期的第一天为Monday，US等每星期第一天为sunday 
     *  
     * @return 
     */  
    public static Date saturday() {  
        return weekDay(Calendar.SATURDAY);  
    }  
  
    /** 
     * 获得周日日期 
     * <p> 
     * 注：日历工厂方法{@link #calendar()}设置类每个星期的第一天为Monday，US等每星期第一天为sunday 
     *  
     * @return 
     */  
    public static Date sunday() {  
        return weekDay(Calendar.SUNDAY);  
    }  
  
    /** 
     * 将字符串日期时间转换成java.util.Date类型 
     * <p> 
     * 日期时间格式yyyy-MM-dd HH:mm:ss 
     *  
     * @param datetime 
     * @return 
     */  
    public static Date parseDatetime(String datetime) throws ParseException {  
        return datetimeFormat.parse(datetime);  
    }  
  
    /** 
     * 将字符串日期转换成java.util.Date类型 
     *<p> 
     * 日期时间格式yyyy-MM-dd 
     *  
     * @param date 
     * @return 
     * @throws ParseException 
     */  
    public static Date parseDate(String date) throws ParseException {  
        return dateFormat.parse(date);  
    }  
  
    /** 
     * 将字符串日期转换成java.util.Date类型 
     *<p> 
     * 时间格式 HH:mm:ss 
     *  
     * @param time 
     * @return 
     * @throws ParseException 
     */  
    public static Date parseTime(String time) throws ParseException {  
        return timeFormat.parse(time);  
    }  
  
    /** 
     * 根据自定义pattern将字符串日期转换成java.util.Date类型 
     *  
     * @param datetime 
     * @param pattern 
     * @return 
     * @throws ParseException 
     */  
    public static Date parseDatetime(String datetime, String pattern)  
            throws ParseException {  
        SimpleDateFormat format = (SimpleDateFormat) datetimeFormat.clone();  
        format.applyPattern(pattern);  
        return format.parse(datetime);  
    }

    //北京时间转为utc时间，oozie 使用
    public static String BJ2UTC(String time) {

        Date date = null;
        try {
            date = parseDatetime(time);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        Calendar cal = Calendar.getInstance( );
        cal.setTime(date);//date 换成已经已知的Date对象
        cal.add(Calendar.HOUR_OF_DAY, -8);// before 8 hour
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm'Z'");
        return sdf.format(cal.getTime( ));
    }

    /*
     * 将时间戳转换为时间
     */
    public static String stampToDate(Long timestamp){
        Date date = new Date(timestamp);
        return formatDatetime(date) ;
    }

    /**
     * 得到最近一个月的所有日期
     * @param d 传入的日期
     * @return
     */
    public static List<String> getDaysMonthByDate(Date d,String pattern)//根据传入的日期获取一个月内的所有日期
    {
        List<String> lst=new ArrayList<String>();
        Date startDate = getOneMonthDate(d);
        while (!startDate.after(d)) {
            lst.add(formatDatetime(startDate,pattern));
            startDate = getNext(startDate);
        }
        return lst;
    }

    /**
     * 得到一个月前的日期
     * @param d
     * @return
     */
    private static Date getOneMonthDate(Date d ){
        Calendar calendar = Calendar.getInstance();
        calendar.setTime( d );
        calendar.add( Calendar.MONTH, -1 );
        calendar.add( Calendar.DAY_OF_MONTH, 1);
        return calendar.getTime();
    }

    /**
     * 当前日期+1天
     * @param date
     * @return
     */
    public static Date getNext(Date date) {
        Calendar calendar = Calendar.getInstance();
        calendar.setTime(date);
        calendar.add(Calendar.DATE, 1);
        return calendar.getTime();
    }


    /**
     * 
     * @param datetime  2017-10-01T16:00:00.000Z
     * @param format  yyyy-MM-dd'T'HH:mm:ss.SSS'Z'
     * @return
     */
    //utc 转换为 北京时间 （时间带有T分隔符的） 
    public static Date UTC2BJ(String datetime, String format) {
	 
		SimpleDateFormat customFormat = (SimpleDateFormat) datetimeFormat.clone();  
		customFormat.applyPattern(format);
		customFormat.setTimeZone(TimeZone.getTimeZone("UTC") );
		
        Date date = null;
        try {
            date = customFormat.parse(  datetime);
        } catch (ParseException e) {
            e.printStackTrace();
        }
 
        return  date;
    }
    
    /**
     * UTC 时间转化为bj时间的字符串
     * @param datetime
     * @param format
     * @return
     */
    public static String UTC2BJStr(String datetime, String format){
    	Date date = UTC2BJ(datetime,format);
    	return formatDatetime( date );
    }
    
    /**
     * 获取某一天的开始时间
     * @param date
     * @return
     */
    public static  Date getDayBegin(Date date) {
         
          Calendar cal = new GregorianCalendar();
          cal.setTime(date);
          cal.set(Calendar.HOUR_OF_DAY, 0);
          cal.set(Calendar.MINUTE, 0);
          cal.set(Calendar.SECOND, 0);
          cal.set(Calendar.MILLISECOND, 0);
          return cal.getTime();
      }
    
      /**
       * 获取当天的结束时间
       * @param date
       * @return
       */
      public static  Date getDayEnd(Date date) {
          Calendar cal = new GregorianCalendar();
          cal.setTime(date);
          cal.set(Calendar.HOUR_OF_DAY, 23);
          cal.set(Calendar.MINUTE, 59);
          cal.set(Calendar.SECOND, 59);
          return cal.getTime();
      }
    	 

    
    public static void main(String[] args) {
//    	String str = formatDatetime("1984-1-1 12:12:13","yyyy-MM-dd");
//        System.out.println(str );
//    	Long t =  millis();
    	
    	String startCreateDate="2017-10-01T16:00:00.000Z";
    	Date start = DateUtils.UTC2BJ( startCreateDate, "yyyy-MM-dd'T'HH:mm:ss.SSS'Z'" );
    	System.out.println( start);
//    	Long t =  1504681347375l;
//    	String str = stampToDate(t );
//    	System.out.println(t+"==="+ str );
//    	
//    	List list = getDaysMonthByDate(new Date(),"M月d");
//        for(int i=0;i<list.size();i++ ){
//        	System.out.println( list.get(i));
//        }		
//        String str2 = BJ2UTC( "2099-12-12 12:12:12") ;
//        System.out.print(str2 );
	}
}

```


 