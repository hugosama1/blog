---
title: Why use EventBus on Android
date: 2017-01-20 16:10:56
tags: 
- EventBus
- Android
- Library
---

EventBus is a small library ( ~50k jar) that allow us to decouple events from Activities and fragments, things like Api calls, Services, Database Access,etc. Can be converted to a publisher - subscriber architecture, as shown in the image below.

![](https://raw.githubusercontent.com/greenrobot/EventBus/master/EventBus-Publish-Subscribe.png "EventBus Architecture")

``` java MainActivity.java
public class MainActivity extends AppCompatActivity {
    private static final String TAG = MainActivity.class.getSimpleName();


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    protected void onStart() {
        super.onStart();
        SendMessages asyncTask = new SendMessages() {
            @Override
            public void sendMessage(final String message) {
                Handler handler = new Handler(MainActivity.this.getMainLooper());
                handler.post(new Runnable() {
                    @Override
                    public void run() {
                        Toast.makeText(MainActivity.this,message,Toast.LENGTH_SHORT).show();
                    }
                });
            }
        };
        asyncTask.execute();
    }

    public abstract class SendMessages extends AsyncTask<Void,String,Void> {

        @Override
        protected Void doInBackground(Void... voids) {
            try {
                for (int i = 0; i < 10; i++) {
                    Thread.sleep(3000);
                    sendMessage("Message: " + i);
                }
            } catch (InterruptedException ex) {
                Log.d(TAG, ex.getMessage());
            }
            return null;
        }

        public abstract void sendMessage(String message);

    }

}
```


``` java MainActivity.java
public class MainActivity extends AppCompatActivity {
    private static final String TAG = MainActivity.class.getSimpleName();


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    protected void onStart() {
        super.onStart();
        EventBus.getDefault().register(this);
        new SendMessages().execute();
    }


    @Override
    protected void onStop() {
        super.onStop();
        EventBus.getDefault().register(this);
    }

    @Subscribe(threadMode = ThreadMode.MAIN)
    public void onMessageEvent(MessageEvent event) {
        Toast.makeText(MainActivity.this,event.getMessage(),Toast.LENGTH_SHORT)
                .show();
    }


    public class SendMessages extends AsyncTask<Void,String,Void> {

        @Override
        protected Void doInBackground(Void... voids) {
            try {
                for (int i = 0; i < 10; i++) {
                    Thread.sleep(3000);
                    EventBus.getDefault().post(new MessageEvent("message : " +i));
                }
            } catch (InterruptedException ex) {
                Log.d(TAG, ex.getMessage());
            }
            return null;
        }
    }

    public class MessageEvent {
        private String message;

        public MessageEvent(String message) {
            this.message = message;
        }

        public String getMessage() {
            return message;
        }

        public void setMessage(String message) {
            this.message = message;
        }
    }

}
```
