
# =====React native =====

# HOC

Full page Spinner on login page
1- Fullpage Spinner comopnent using as HOC and this use in Login component
##### FullScreenSpinnerHOC.js

```
import React from 'react';
import { View, StyleSheet, ActivityIndicator } from 'react-native';

export default (Comp: ReactClass<*>) => {
  return ({ spinner, children, ...props }: Object) => (
    <View style={{ flex: 1 }}>
      <Comp {...props}>
        {children}
      </Comp>
      {spinner &&
        <View
          style={[
            StyleSheet.absoluteFill,
            { backgroundColor: 'rgba(0, 0, 0, 0.7)', justifyContent: 'center' }
          ]}
        >
          <ActivityIndicator size="large" />
        </View>}
    </View>
  );
};

```
##### Login.js

```
// @flow
import React, { Component } from 'react';
import {
  View,
  Image,
  StyleSheet,
  TextInput,
  TouchableOpacity,
  Text,
  KeyboardAvoidingView
} from 'react-native';
import * as images from 'src/images';
import * as HOC from 'src/HOC';     //FullScreenSpinnerHOC is in HOC folder ,

const DismissKeyboardView = HOC.DismissKeyboardHOC(View);
const FullSCreenSpinnerAndDismissKeyboardView = HOC.FullScreenSpinnerHOC(
  DismissKeyboardView
);

const KeyboardAwareImage = HOC.KeyboardAwareHOC(Image);
const KeyboardAwareView = HOC.KeyboardAwareHOC(View);

export default class Login extends Component {
  state = {
    logging: false
  };
  // This is for demo only, normally you want to create an api wrapper
  async callLoginAPI() {
    this.setState({ logging: true });
    await new Promise(resolve => {
      setTimeout(resolve, 2000);
    });
    alert('SIGN IN success');
    this.setState({ logging: false });
  }
  render() {
    return (
      <FullSCreenSpinnerAndDismissKeyboardView
        spinner={this.state.logging}
        style={styles.container}
      >
        <KeyboardAvoidingView behavior="padding" style={{ flex: 1 }}>
          <KeyboardAwareView
            style={{
              flex: 1,
              marginBottom: 40
            }}
            styleDuringKeyboardShow={{ marginBottom: 10 }}
          >
            <KeyboardAwareImage
              resizeMode="contain"
              style={[
                {
                  height: '100%',
                  width: '100%'
                }
              ]}
              styleDuringKeyboardShow={{ opacity: 0.5 }}
              source={images.gembul}
            />
          </KeyboardAwareView>
          <TextInput placeholder="Username" style={[styles.textInput]} />
          <TextInput
            placeholder="Password"
            style={[styles.textInput, { marginVertical: 20 }]}
          />

          <TouchableOpacity
            onPress={() => {
              this.callLoginAPI();
            }}
            style={[styles.button]}
          >
            <Text style={{ color: 'white', fontSize: 20, fontWeight: '600' }}>
              SIGN IN
            </Text>
          </TouchableOpacity>
        </KeyboardAvoidingView>
        <TouchableOpacity
          style={{
            alignSelf: 'flex-end',
            height: 40,
            justifyContent: 'center',
            marginBottom: 20
          }}
        >
          <Text style={{ color: '#BDC3C6', fontSize: 15 }}>
            Need Help?
          </Text>
        </TouchableOpacity>
        <Text style={{ alignSelf: 'center', color: '#A6A8A9', fontSize: 15 }}>
          Don’t have an account yet ?
        </Text>
        <TouchableOpacity
          style={{
            alignSelf: 'center',
            height: 34,
            justifyContent: 'center'
          }}
        >
          <Text style={{ color: '#0D92CA', fontSize: 15 }}>
            Create an account
          </Text>
        </TouchableOpacity>
      </FullSCreenSpinnerAndDismissKeyboardView>
    );
  }
}
const styles = StyleSheet.create({
  container: {
    flex: 1,
    paddingHorizontal: 26,
    paddingTop: 26,
    paddingBottom: 18
  },
  textInput: {
    height: 60,
    borderRadius: 3,
    borderWidth: 1,
    borderColor: '#ECF0F3',
    paddingHorizontal: 19
  },
  button: {
    height: 60,
    borderRadius: 3,
    backgroundColor: '#11B8FF',
    justifyContent: 'center',
    alignItems: 'center'
  }
});
```

###### HOC folder
 in HOC folder 4 files are ther three is HOC and one is index.js below code for 
 ##### index.js
```
// @flow
import FullScreenSpinnerHOC from './FullScreenSpinnerHOC';
import DismissKeyboardHOC from './DismissKeyboardHOC';
import KeyboardAwareHOC from './KeyboardAwareHOC';

export { FullScreenSpinnerHOC, DismissKeyboardHOC, KeyboardAwareHOC };
```






# ======Android=======


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


# Services Behaviour
   START_STICKEY == 1-> Services are being explicity managed and long running
                  2-> No need to remember state at kill time
                  3->Long running music plaing service
                  
   START_NOT_STICKEY == 1-> Services are being not explicity managed
                        2-> Service are periodically running and self stopping
                        3-> Alarm services or server data polling
                        
                        
   START_REDELIVER_INTENT == 1-> Services are being explicity managed
                            2-> Resart from previous state at the kill time
                            3-> File download

