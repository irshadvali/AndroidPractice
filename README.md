# AndroidPractice
public class HttpRef {

    //Test
    public final static String domain = "http://myjson.com/";




    public final static String path = ":3000/";
    public final static String imagepath="http://myjson.com/test/uploads/";
    public static OkHttpClient okClient = new OkHttpClient.Builder()
            .addInterceptor(
                    new Interceptor() {
                        @Override
                        public Response intercept(Chain chain) throws IOException {
                            Request original = chain.request();

                            // Request customization: add request headers
                            Request.Builder requestBuilder = original.newBuilder().method(original.method(), original.body());

                            Request request = requestBuilder.build();
                            PrintO.log("GET CALLS : "+chain.request().url().toString());
//                           System.out.println("Request__: "+request.headers().toString()+" : "+request.body().toString()+" : " +chain.request().headers().toString()+" : "+chain.request().body().toString());
                            return chain.proceed(request);
                        }
                    })
            .build();

    public static Retrofit builder = new Retrofit.Builder().
            baseUrl(domain).addConverterFactory(GsonConverterFactory.create()).client(okClient).build();
    public static ApiService createService(){
        return builder.create(ApiService.class);
    }

//    public static OkHttpClient interceptClient(){
//        OkHttpClient client = new OkHttpClient();
//        Interceptor interceptor = new Interceptor() {
//            @Override
//            public Response intercept(Chain chain) throws IOException {
//                Request request = chain.request();
////               System.out.println("GET CALLS : "+chain.request().urlString());
////                AppDataStore.triggerAction(AppEvent.API.toString(), chain.request().urlString());
//                return chain.proceed(request);
//            }
//        };
//        client.interceptors().add(interceptor);
//        return client;
//    }



}



///==================//

# AsyncTask


AsyncTask enables proper and easy use of the UI thread. This class allows you to perform background operations and publish results on the UI thread without having to manipulate threads and/or handlers.

AsyncTask is designed to be a helper class around Thread and Handler and does not constitute a generic threading framework. AsyncTasks should ideally be used for short operations (a few seconds at the most.) If you need to keep threads running for long periods of time, it is highly recommended you use the various APIs provided by the java.util.concurrent package such as Executor, ThreadPoolExecutor and FutureTask.

An asynchronous task is defined by a computation that runs on a background thread and whose result is published on the UI thread. An asynchronous task is defined by 3 generic types, called Params, Progress and Result, and 4 steps, called onPreExecute, doInBackground, onProgressUpdate and onPostExecute.





    private class DownloadFilesTask extends AsyncTask<URL, Integer, Long> {
     protected Long doInBackground(URL... urls) {
         int count = urls.length;
         long totalSize = 0;
         for (int i = 0; i < count; i++) {
             totalSize += Downloader.downloadFile(urls[i]);
             publishProgress((int) ((i / (float) count) * 100));
             // Escape early if cancel() is called
             if (isCancelled()) break;
         }
         return totalSize;
     }

     protected void onProgressUpdate(Integer... progress) {
         setProgressPercent(progress[0]);
     }

     protected void onPostExecute(Long result) {
         showDialog("Downloaded " + result + " bytes");
     }
    }

Once created, a task is executed very simply:

new DownloadFilesTask().execute(url1, url2, url3);
