[TOC] 



#### 1.TimeoutException
    
    java.util.concurrent.TimeoutException:         android.content.res.AssetManager.finalize() timed out after 120 seconds
    at android.content.res.AssetManager.destroy(Native Method)
	at android.content.res.AssetManager.finalize(AssetManager.java:576)
	at java.lang.Daemons$FinalizerDaemon.doFinalize(Daemons.java:217)
	at java.lang.Daemons$FinalizerDaemon.run(Daemons.java:200)
	at java.lang.Thread.run(Thread.java:818)
解决方法

    public void fix() {
        try {
            Class clazz = Class.forName("java.lang.Daemons$FinalizerWatchdogDaemon");

            Method method = clazz.getSuperclass().getDeclaredMethod("stop");
            method.setAccessible(true);

            Field field = clazz.getDeclaredField("INSTANCE");
            field.setAccessible(true);

            method.invoke(field.get(null));

        }
        catch (Throwable e) {
            e.printStackTrace();
        }
    }