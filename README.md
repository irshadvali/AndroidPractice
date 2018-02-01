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
